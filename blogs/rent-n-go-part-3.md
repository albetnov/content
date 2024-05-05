---
title: Rent N Go (Part 3)
description: The final piece from the previous Rent N Go (Part 2) Blog. Rent N Go. The second iteration of our Expo App. Rent-N-Go offers a variety of services for car rental. It features Car Rental, Driver, and Tour. This blog is about the creation of Rent-N-Go, why Car Rental, how we build it, why it uses Go and many more.
categories: [3]
collections: [3, 2]
created_at: 2024-05-05
updated_at: false
archived: false
---


# Rent N Go (Part 3)

:cover-image{src="/blogs/ttngcd7za0ynkepc3rmf.jpg" alt="Rent N Go (Part 3)"}

## Quick Summary

Previously in the [Rent N Go Part 1](/blogs/rent-n-go-part-1) I shared about how the project will be structured and what kind of tech stack is used on the project (Fiber, GORM, React). I also mentioned the app we're building Rent N Go a car rental with additional services such as Driver and Tour.

In [Rent N Go Part 2](/blogs/rent-n-go-part-2) I shared more technical detail on how the REST API in the backend was created, what are the challenges, and more. I also shared the topic of how we handle validation, image upload, and how we implement the front end, including stuff like creating an order page, the caveats, etc.

<!--more-->

## Challange in Developing Frontend

Are there any challenges we encounter when developing the front end using React? The answer is obviously, yes. Luckily, though not so much like the previous app [BodyFits](/blogs/bodyfits). The first challenge is fetching data. In React, if you use `useEffect` to fetch initial data, you mess up *kinda*. I mean even the React Docs itself discourage you from using `useEffect` manually, it's for a good reason though look at [Escape Hatches](https://react.dev/learn/you-might-not-need-an-effect#fetching-data). There are multiple states you should consider managing, like `ignore`, `error`, and `pending`. So it is tedious, and this issue seems to not only appear on React.

### 1. Fetching Data

Fetching Data in React is tedious. For initial data fetching we use Loader from React Router. It's pretty good, though we still have no idea how to refetch? should we refresh the page or anything? Any other API calls are made directly using `axios` and its interceptor. Because of that, we need to manage some edge cases. But no big deal. The problem only arises when we have a component and not a page. This component is mounted dynamically, and when it mounts, we want the component to fetch data. We use `useEffect` for that and the effect runs twice! The issue can be fixed using a library such as [Tanstack Query](https://tanstack.com/) or [useSWR](https://swr.vercel.app/).

### 2. Guarding is Hard

React Router didn't have middleware like in another router of a different library (say Vue Router). Really good experience. Anyway, in React Router you need to make a component to become the parent. Where this component will run on the client and execute any required logic return error or redirect component upon error, and return `<Outlet />` upon success. Pretty simple, until you have to fetch. Ah, we go back to [1. Fetching Data](#_1-fetching-data).

> There might be other various challenges, but it might not be a big deal and probably I have forgotten it tho.

## Developing the Admin Panel

Enough about the Frontend challenge. It's a pain. A nightmare that I don't want to remember, but it is necessary to grow. Thanks, React. Now what about the Admin Panel? We can build the Admin Panel in React *again*. But considering the previous iteration, where a lot of mess came from the Admin Panel we chose to just build the panel traditionally. No CSR, SPA shit just directly. So GOHTML here we go!

This is quite interesting, GO has a built-in templating language. I think it's called GOHTML? or Go Templates? I don't know let's just simplify and call it GOHTML. So this GOHTML is pretty powerful. It shares similarly with other templating languages having `{{ Var }}` to output the HTML. The main part of this template is called Actions and Variables. In GOHTML you can assign a variable like this:

```html[example.gohtml]
{{ $text := .Description }}
<p>{{ $text }}</p> {{ /* BLADE IS THAT YOU? */ }}
```

You can also have your custom template functions. The problem:

### Custom Functions
You can't embed the Go Code right in the GOHTML. So if you need certain parsing you either have to do it when you pass the context/data to the view or you have to build the custom function if you can't manipulate your context/data. For example in certain places, you want the data to be formatted while in others showing raw. So it's quite cumbersome having to add several custom functions. Alternatively, we can duplicate the data and pass it to context to something like this:

```go[example.go]
SomeStruct{
  Data: {
    Date: 1714747051478
  },
  FormattedData: {
    Date: "03/05/2024"
  }
}
```

The caveat of the approach above is that you deliver twice the amount of data because you need to format them, and then if it were in the array, well, things get more cumbersome. You might end up having the redundant formatted data. So it will be nice to embed the Go in the HTML. There is a library that does that when I was writing this I stumbled open [Templ](https://github.com/a-h/templ). This brings HTML to the Go Code. So you will embed HTML code inside Go Code. Much like JSX. Fair enough, as long as I have access to both languages and no need to undergo some custom function and syntax bla bla bla.

### Lack of Error Traces

In GOHTML, when you make a mistake. You will only have 1 trace. And that trace is rendered as a string. A P Tag maybe? 1 Trace is quite tedious, right? moreover, if the traces are incorrect or maybe somewhere in the vendor we can say the trace is not right on target. But fair enough, we can debug and manually check for each execution. The thing is when you have complex and many Go HTML Views included in your main template let's say it's a component, you might notice that your component is not rendered and is an error because the error is rendered as normal HTML.

It didn't interrupt your other views and everything looks fine until you noticed the error text. This, combined with some amounts of CSS and overflows might potentially lead the error to be hidden until you look at the source of the view or look manually at the response.

If only there were options for the configuration of the template to show an error like Laravel Ignition which replaces the entire page or Next.js Modal which overlays on top of your page, allowing you to quickly notice the error right away. Alternatively, perhaps print it to the cli output?

## The Expo Day

After all of the challenges, here comes the Expo Day. This Expo compared to the previous one has also invited Industries to join and check our work. Honestly, from the Expo I have encountered until now, the Second Expo is by far, for me, the most boring one. The lecturer came and verified our work, including our Animation 2D, which is great btw. Nothing really special on this day worth remembering.

Apart from having industries, we don't really catch their attention, and they seem to be ignorant and just keep walking without standing in front of any booth. Well again, acceptable, they have standards. We understand. Perhaps our project does not correlate with their industry focus. But again, I must say this Expo is completely boring. You don't even have to read about this section at all, because this section most likely will contain unrelated murmur.

And as for our project? Well, I happen to be in a meeting being a Student Association after all. So I can't be present when it's time to showcase our Web Project, yes the entire Rent N Go. I can't attend. Turns out, from what I received from my friends. We don't have that much time to present the Web Project (Rent N Go) because of limited time. A lot of guests and visitors also already leave. So yeah all of those efforts, for nothing. At least I got a decent score. Thanks for reading this blog.