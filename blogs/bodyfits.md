---
title: BodyFits
description: The Story behind BodyFits. Our first Expo Software.
categories: [3]
collections: [2]
created_at: 2024-04-06
updated_at: false
archived: false
---


# BodyFits

:cover-image{src="/blogs/mhvrxtmfm2vinqoajrbn.jpg" alt="BodyFits"}

> Building the body of BodyFits, for the better fit of today's fitness software.

That was our slogan "so to speak", about our App BodyFits. In short, BodyFits is like any other app you see out there (about fitness) but it scales down to what you can expect from college students doing things necessary
just to get the project done lol.

So without further ado, let's begin...

<!--more-->

## The First Semester

The first semester, as you can guess most students are motivated, right? Since this is you know the first semester! Where every high school student finally goes to college level up from their previous study. It's only normal they get motivated after all this is the anticipated moment ~~unless they are forced to enter college, just kidding~~. So in the first semester, most of the courses are just __introducy__ kind of sections, so in this course, we're not going into
more depth regarding the course. Some important courses in the first-semester correlate with Computer Science, these are:

- Data Structure & Algorithms
- English
- Programming Technique
- Multimedia
- UI

Upon this course, the lecturer also tells us that there is a semesterly (6-month once) event on my campus, and the name is...

## Expo

Expo ~~not Expo React Native~~. But Expo [Exhibition](https://en.wikipedia.org/wiki/Exhibition) is a regular 6-monthly event where each student presents their project either as a group together or individually (I rarely see individuals though). So each group containing students varies from 4-6 members and different semesters, all present their final semester project. So basically, you can expect many projects from the web, apps, games, videos, short movies, etc.

In the first semester, we were given a task to create any apps we wanted. Of course, most of us choose to create a web because it's beginner-friendly. However, I won't deny there are some experts you know those who code in a hoodie and casually make a mobile app or game. In just 6 months. I am impressed.

Anyway, what is the theme or the web app we decided to create eventually?

## BodyFits

As you might guess from the title, *it is* BodyFits. A fitness-focused software where it contains features like:

- Trainer Management
- Videos / Text Resources
- Progress Tracker
- Random Recommendation (That not using any algorithm just completely random)
- etc... (maybe?)

If you are interested in the project itself, I have open-sourced it in the first place so you can give it a star perhaps?
[BodyFits](https://github.com/albetnov/bodyfits-inertia).

### How did we get here?

Well, the reason why we decided to go with BodyFits, is simply because the lecturer of UI class told us to look for a UI sample on the web for us to take inspiration for, and I simply ran into this dribbble (is that how you write them?) page where it's shows a great resemblance of black and green to build a fitness software. It's a screenshot of a mobile app actually but somehow we managed to get inspiration from that and build the responsive design on our own. So now, we have decided what we're gonna make and the UI is on the go, the next step is...

## Building BodyFits

So now that we know what we're gonna make, the next step is to choose the right tool to build a website about Fitness. So I begin the research with the following questions:

- What are the right tools for beginners?
- How steep the learning curve is?
- Should we build from scratch?
- Is the time enough?

1. So let's answer the first question what are the right tools for beginners?
Well, to learn the web. I assume only HTML + CSS + JS is enough. Because these tools all are you need to build a web app, with just a static app perhaps HTML and CSS alone are already enough. So it's decided. 1 Programming Language and 2 Markup Language. At the same time, I gave them a boot camp which is quite good, so they should've managed it somehow right?

2. How steep the learning curve is?
With the Bootcamp, it should not be that steep right? The only struggles might be on the JS part.

3. Should we build from Scratch?
The problem with the learning above languages is that most of us are only limited to having the ability to write a web from complete scratch, which in our case could take a long time.

4. Is the time enough?
It's not. If we had started planning earlier, we might. But in the end, we decided to build the web when only 3 months left.

So with all of these questions answered, we pick one conclusion, that is using [React](https://react.dev). I know. I know. React might not be the best tool to learn when you still somewhat lacking in fundamentals. Luckily though I have experience (not a lot) with React and have been building a web app before using it, so I do most of the setups first before they can then contribute. Why? because we have a custom UI that needs [Chakra UI](https://chakra-ui.com/) to quickly craft the UI and Chakra UI is React Specific only. While we can build it from just HTML and CSS our time is somewhat limited. Because both the design and implementation are implemented parallelly. 

Besides that, we also need a way to manage the UI with some sort of Layouting, which in React we can do it easily in HTML-Like format using [JSX](https://react.dev/learn/writing-markup-with-jsx). Now that we have chosen React the next step is the Backend/Framework:

### MeteorJS

When I was looking for a full-stack framework, I found [MeteorJS](https://www.meteor.com/learn) appealing. The reason is that the `useTracker` and its ecosystem (with Mongo) allow me to easily integrate the backend and frontend back and forth. MeteorJS supports many front-end frameworks to build the UI (React, Vue, Svelte, Blaze).

So looking at the framework it's almost the perfect fit, however, the supported React at that time was still unfortunately limited to version 17 only, whereas Chakra UI requires Version 18 of React.

### Next JS

I do have a plan for this but quickly canceled it, considering how complicated stuff such as SSR and stuff. At the time of building the project [`app/` dir](https://nextjs.org/docs/app/building-your-application/routing#the-app-router) was also introduced but still in beta.

### Laravel & Inertia

Yes, [Laravel](https://laravel.com) is [PHP](https://php.net) another +1 programming language, but hey I have more experience with PHP and Laravel compared to the other two stacks I mentioned above. This is at the end of the stack we chose where I mainly worked on PHP and JSX Components abstractions and my friend would place the components and design the page according to the UI.

Using Laravel and [Inertia](https://inertiajs.com/) allows me to be more productive and quickly build many features in a short time. However, not the same case for my friends as they have to set up PHP, Composer, and eventually MySQL.
In summary, using [Laragon](https://laragon.org) is already enough, but some of them have troubles in port and such stuff, and also still quite cumbersome because you have to run some `artisan` commands. In the end, I made a bash script to automate stuff but then they used Windows which requires WSL and WSL activation settings as well. 

## Evaluation

1. We should plan our steps quicker and start going to UI and the Web quicker too, so that we can have more deadlines ahead. 
2. We should pick another tech, aside from Chakra UI maybe pick something more HTML-ish such as DaisyUI or another components library, thus eliminating the need for React.
3. If I had chosen Node in the first place, we could have eliminated the need for PHP, and perhaps with the usage of JS, my friends could also benefit themselves by learning the code although they did not do Backend.
4. Use SQLite instead of MySQL and we could have eliminated the need for Laragon / MySQL.
5. Integrate post-install commands so the package manager can take care of setting up stuff automatically.

## Conclusion

In the end, building BodyFits was a indeed valuable experience for me and the rest of my friends, we learned about when should we plan and build something, we learned about choosing the right tool, picking the right time, and other considerations.

Thanks for reading this blog. This blog served as my diary. Sorry for my bad English too :v.