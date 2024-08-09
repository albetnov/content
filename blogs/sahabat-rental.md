---
title: Sahabat Rental
description: The third iteration of the final semester app is to be showcased at the Expo. Meet, Sahabat Rental. It is the same app with the same theme as before but with a different purpose.
categories: [3]
collections: [2]
created_at: 2024-08-09
updated_at: false
archived: false
---


# Sahabat Rental

:cover-image{src="/blogs/ajxdedzkrk9cj45snmk7.jpg" alt="Sahabat Rental"}

Yet again another final semester app will later be showcased at the Expo. Sahabat Rental. Like the thumbnail, this app is about the Financial Management of a Rental Service Provider. Consider it an add-on to our main app or a spin-off or something. The app will mainly consist of fields, data, and reports. Not really an app to be presented or used by the customer rather than by the Car Rental Provider themselves.

<!--more-->

## Quick Look

![Sahabat Rental](https://www.albetnv.me/_vercel/image?url=%2Fprojects%2Fsr_home.webp&w=640&q=100)

As you can see in the glance above. Sahabat Rental is a financial management app made specifically to cover Car Rental only.

## Chapter 1: Why Sahabat Rental?

Last semester, we decided to pick Car Rental for our future reference in building an app for campus assignments in the future, and this is also the same reason why we decided to make the financial app for it. So in the third semester, we have a course about accounting so that's why we have to build an app based on the course. The project itself is the final project of joint courses between accounting, web, and database courses. Because of that, we consider creating a financial app for rental management will be great. So then, the journey begins!

## Chapter 2: Understanding The Requirements

We have first to focus on the target and how the app gonna shape before actually building. Who will be the customers? can everyone just directly be our customer? Will we host the entire infrastructure and users just use them? or should we make it specific?

Considering the requirements of passing the current semester, we had to work together with other businesses. This means we already have our customers, we will only have one customer and thus, Sahabat Rental *will be a fully customized app made for them*. Now that we have it figured out, the next is how we handle the access? since the app is customized and built for one customer only then only their internals would use them, therefore we decided to just have a single role and landing page. Since the app is also mostly about finance meaning most of the UI will consist of forms and input. Therefore, we can stick to a simple admin design.

Most of the contents will also have minimal interactivity so we can eliminate the need to use interactivity frameworks/libraries like React, Vue, etc. Let's just stick with my favorite framework, Laravel, and my buddy Blade. Since we still need a bit of interactivity we will add AlpineJS for a bit of spice. Little but powerful. Then for the content, we decided to cover features such as car (including variants/colors), customer, stock, transaction (earning & expense), and finally report. So because of our features, a lot of our data will likely to tightly coupled (in relationships and such) so we need a powerful ORM like Eloquent built-in baby.

So far, we managed to conclude the requirements:
- Simple UI (forms focused)
- Minimal Interactivity (no need for additional JS library and their node_modules)
- Tightly connected data

and as I already mentioned many times, Laravel is a fitting role for this as I don't have to set up painful stuff like connecting ORM, creating API, creating migration and seeding interface, configuring bundles, pain, pain, and many other pains.

## Chapter 3: Building The App

Building the app is straightforward, what is not is my knowledge about business finances (moreover in car rental), and accounting. Yes, I am clueless. I have no idea how to make a proper report! I am clueless! There are also minimum criteria where of reports our app needs to make around 3: balance, cash flow, and one more thing that I forgot. Also, there are account numbers? and other accounting stuff that I don't know about.

So for a few weeks, I decided to create the car module with colors where it has a separate stock. Also, the stock management is where each of the stock movements is being recorded there, I honestly think the stock management has some flaws in the flow but I can't remember them. Typical. With the power of Blade Components + Alpine, I enjoyed a swift and smooth development and easily achieved what I wanted, of course without hydration issues. Just kidding.

Combine them with an Eloquent relationship and bam I got the Stock Movement and the car finally connected (also the colors). Nice. A good start. Now that the baseline is complete, the next thing to create will be the financial feature and that is transaction. I decided to split the transaction into two parts Expenses and Earnings so ~~ that we have more menu in the sidebar!~~ I can easily manage them since they have their own table anyway. The earnings will be connected to the order which includes the car while the expense is not. It's quite easy to create them since they are similar to the car module after all. But I removed the ability to edit because ~~it's cumbersome to work for~~ most transactions made should stay as is.

Then what about the report? well, my lecturer decided to ease up the difficulties where the app should be as easy to use even for those with no accounting background. So that means, I can create the app as I used to! nice! I don't have to implement the sequel of Accurate.

## Chapter 4: The Code Review

In this project, I prefer to experiment with something that I am still lacking in Laravel Framework, including learning new features that are available for Laravel 10 and some that I haven't tried yet.

### The Blade Components

The Blade Components are powerful and appealing features of Blade in my opinion. The syntax for the markup is quite similar to Vue. You would declare a component using `<x-name>` where you change the name to the file name of your component file. You can pass the props as how you do it with HTML but use special syntax like this `:attribute` to pass PHP value within it. Nice!

The component also already supports attribute inheritance so all you need to do is just `{{ $attributes }}` and that's it. You can even merge them or apply conditional classes! how convenient! With that, I created some components like alerts, auth, layouts, button, card, and many many more.

### Macroable Trait

In almost Laravel API they usually include the `Macroable` trait. This trait is powerful. It's like you can mixin a class right in runtime! However, it does lacking the IDE support and documentation as you would have to edit the phpdoc in the vendor folder directly which is just not recommended. Don't do that. Please don't push your vendor to git.

I take the chance to save some boilerplate so that I can do `back()->alert()` by just code like this:

```php
RedirectResponse::macro('alert', function (string $status, string $message) {
    if (!in_array($status, ['success', 'error', 'info'])) { // info is added here
        throw new Exception("Invalid $status property");
    }

    return $this->with('alert', compact('status', 'message'));
});

back()->alert(); // since back is instance of RedirectResponse
```

### Vite is noice

Vite is really convenient, to bundle all my CSS and JS assets I just need to do (combined with `laravel-vite-plugin`!):

```js [main.js]
import "./auth";
import "./app.min.js";
import "./libs/simplebar/dist/simplebar.js";
import "notyf/notyf.min.css";
import { Notyf } from "notyf";
```

and a bit of input:

```js [vite.config.js]
import { defineConfig } from "vite";
import laravel from "laravel-vite-plugin";

export default defineConfig({
    plugins: [
        laravel({
            input: [
                "resources/css/app.css",
                "resources/js/main.js",
                "resources/js/auth.js",
                "resources/css/landing.css",
                "resources/js/dashboard.js",
            ],
            refresh: true,
        }),
    ],
});
```

### View Composer
View Composer is a saver to help me reduce many duplications of page meta-data. Such as breadcrumbs and titles.
This allowed me to keep the focus on providing the data that I really needed rather than having to pass each page meta-data on each page available.

### Translation

The app also comes with Indonesian, English, and Chinese Mandarin features taking advantage of `app()->setLocale` and `app()->getLocale`!

That's all for the code reviews!

## The Expo Day

Finally, on the day of the Expo where as usual we introduce our app to the guests from outside and the campus itself. This Expo is different than the previous one! since we don't have to present every time! We can just present when it's our time to showcase! A lot of time-saver and boredom-saver! Considering how boring it is previous Expo was I hope this one is not!

So, knowing that the Expo started at 2 PM at least for our group, I just woke up earlier and enjoyed some games before the expo finally started.

The sky is sunny, the morning atmosphere feels so good and of course, I know it by guessing and like instinct because I am not leaving my house so early lol. Fast forward I played one armed robber together with my friends until the clock showed 12.30 PM. It's time to go to campus for the Expo, but guess what? it begins raining!

So I just halt it for a few minutes! Later, after arriving we quickly set up a bit, and the clock shows 2 PM. The session is now shifting to Third Semester Projects and things started up quite great, the website functions as it should, though we do find multiple bugs (why always like this? I swear we have checked thoroughly together last time!):

- The sidebar is not scrollable on screen with fewer pixel densities (I guess?)
- Unable to create a report when there’s no data at the selected date range.

I managed to fix the issue on the same day quickly and locally first. After that, we eventually met some of the guests who were awesome in my opinion! Giving us some advice and improvement for the future! Some of the guests also told me to choose my words more wisely (not in bad terms) like instead of saying parallelism I should say concurrent.
The sidebar is pretty easy just add overflow:

```css
.sidebar {
// …
  overflow-y: auto;
} 
```

As for the report, the issue is pretty simple, we have some types errors:
```php
// Models/Earnings.php && Models/Expenses.php
public function price() {
	return new Attribute(fn($value) => rupiah($value)); // rupiah expects `int|float` here.

  // the fix
  return new Attribute(fn($value) => $value ? rupiah($value) : $value);
}
```

and add some guard before generating the report:
```php
if(count($earnings) <= 0 || count($footers)) {
	return back()->alert(‘error, ‘No data is available within that range’) ;
}

$report->generate(…);
```

and yes, the fix still lacking. That's why yet another bug was found by our guests! Wow, I should have brought my other project to have competent QA's like them.

Finally, the lecturer came to visit us. The first lecturer said that the app is quite slow, The second lecturer recommended us to put a placeholder of a date range selection in reports generation. After that with some free time, I and my friends decided to visit other work where suddenly a guest visited our tables so we had to rush back. We even haven’t reached the ¼ yet and suddenly a guest visited our tables.

And that’s all the bugs and the fixes we have encountered along with the Expo. As time passed by, we met some guests asking more about technical questions:

-	What are the tech stacks?
-	Why you choose Laravel?
-	Why you choose Go when there’s already Laravel?
-	What do you mean by ‘Parallel’, is ‘concurrent’. they are different?

These above are not a bad thing, it’s reasonable questions. Along the way, I also get some recommendations like:

-	You can fix the sidebar by adding `overflow-y` and I did that (above).
-	Perhaps you should consider adding an English Language option, and I did that (after the Expo).
-	Car monthly installment tracking (+features)
-	Asset tracking (+features)

And of course, some critics:
-	The app is quite slow.
-	Learn more about how to unleash more power of Go.
-	Is Go really needed in this app?

And then some things that I want to add by my own self is about (I have not do anything about this. I do plan it. But hey! it remains a plan haha):
-	Car edits on stock should trigger a stock movement automatically.
-	Add rollback on stock movement.
-	Add rollback on earnings.
-	Add rollback on expenses.
-	Track the generated reports and perhaps cache them.

let’s answer some questions that are related to the current decision.
-	What do you mean by ‘Parallel’, is ‘concurrent’, They are different.
That’s true, I happened to read about this in a random blog and of course the Stack Overflow! The top answer is clear and easy to understand already. They are quite similar but different. In summary concurrent allows a task to run together, but not necessarily at the same time, and they can finish at any time either. Parallelism on the other hand allows the task to be run at the same time.I thought explaining Parallelism is much easier to understand for those who are not a Developer, but I guess I should choose my words better next time.

-	Why still using Go when there’s already Laravel?
Because of the specific task that the course requires. I know that I can just use Laravel Queues to achieve the same non-blocking stuff though it did involve more setups. Also, PHP has fibers now which I can easily resume and suspend at any time.

-	The app is quite slow.
It is. Unlike the previous app where we used React as the frontend and data transfer was done via API, the UI also has skeleton loading indicating that the UI is ready, but the data is still loading, also it’s SPA. So, navigation is handled on client side and fetch over new components only when needed.

Although the SEO might struggle a bit, it can be solved by adding the SSR. Yeah! However, we have limited time and I think Laravel + Blade + AlpineJS is already enough though changing routes is quite slow due to how much asset need to be resent over and over per-page switch!

That's all! What a lengthy blog!
