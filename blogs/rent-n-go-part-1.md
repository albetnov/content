---
title: Rent N Go (Part 1)
description: Rent N Go. The second iteration of our Expo App. Rent-N-Go offers a variety of services for car rental specific. It features Car Rental, Driver, and Tour. This blog is about the creation of Rent-N-Go, why Car Rental, how we build it, why it uses Go, and many more.
categories: [3]
collections: [2]
created_at: 2024-04-20
updated_at: false
archived: false
---


# Rent N Go (Part 1)

:cover-image{src="/blogs/ecnhznchhu7pbusqy1mo.jpg" alt="Rent N Go (Part 1)"}

Rent N Go, a second iteration app for Expo Showcase.

In this second season/iteration of our Expo Showcase. We decided to create a car rental app that focused on Car Rental, Driver Services, and Tour Assistance. The app will use Go as the Backend alongside Gin as the Web Framework and Gorm as the ORM. The front end will be using barebones React and Vite setup combined with React Router to navigate between pages and Axios for the API Client.

So let's split the story into multiple chapters, shall we?

> This is a very long blog some of its content contains my campus story, if you just want to go to the app, skip to [Chapter 3: Rent N Go](#chapter-3-rent-n-go)

## Chapter 1: The New Class and New Environments

Most of the students on my campus would most likely stay in the same class number as they progressed their grades for the next semester ahead. This means in the second semester of each of the classes.

<!--more-->

Most of the students will remain the same (from the previous semester). So at that moment, you can say we're pretty energized and wanted to explore more, try new environments, try another class number, etc. So here come the random wheels to decide which class we will be going to. There we listed several class numbers:
- 2A
- 2B
- 2C

You can think of them looking like this. Next, let's spin the wheel! As the wheel keeps spinning and spinning, each of these numbers candidates gets eliminated and eliminated... Until we concluded.

A new class and a new environment are waiting for us. There are some days/weeks (I don't remember it) before we get back to Campus and do college student activity that is not boring, always fun, challenging, and many more! What an exciting day indeed! In the new class, however, we also have newer people to introduce and yeah you know the details. It's quite difficult to get used to each other there, mainly because we're like a newcomer to an already established circle or something. But, it's fine they treat us pretty well too besides those quirks.

We moved to that class together with my friends. In total 7 people. and because of that in the end we build a group consisting of 7 members. Of course, some courses can't accept such numbers. So we end up splitting in half and have 2 random people in that class joining us as our members. Welcome abroad! Now that we have concluded about the new class and how many members are specific in Web Building, we're ready to move to the next chapter!


## Chapter 2: What We're Expect to Build?

Just like the previous semester, in this semester a final project is also required to pass the entire semester. So in the beginning, each lecturer filled in what project we should create. This semester, the Design aspect of Computer Science introduced stuff such as 2D animation (which I am quite bad at) and Web Design (which surprisingly focused on the code more, I thought we would work in Figma?). So as you can expect our project is related to 2D animation and the Web again, ultimately.

Well, it's fine! I am not complaining at all considering that I am a Web Developer after all. In fact, I am happy because I don't have to explore any other unknown stack to build another app (mobile, desktop, etc)! So this semester too we're free to choose any tech stack we want, while for the 2D animation, we are encouraged to use Adobe Animate. We've been struggling to build the 2D Animation as none of us were proficient in using Adobe Animate lol. So yeah, we built the project but you know its movement is rigid, the animation is simple, and even some voice and assets are not in sync. But hey! I think it's pretty well since none of us were into 2D Animation still can produce such results. We try you know.

Now enough about 2D Animation mainly because I am not working a lot in this course, you can say we decided to split each of our members tackling tasks focused on different courses. So for this, my guy [@AtnanAriAnderson](https://github.com/AtnanAriAnderson) is the lead project of 2D Animation. He knows more detail about what's going on in 2D Animation compared to me (I only work as an Editor there, using Capcut. The best software editor. It's much simpler and more productive compared to Premiere Pro when editing simple stuff).

Apart from 2D animation we also have some other final exam or projects those are Networking and a simple web project. So this simple web project differs from the final project (that is web too) think of it as a project from a separate course. This simple web project focused on exploring and creating a simple CRUD web that uses native without a framework, etc. I finished the project using PHP and it was a breeze. Networking on the other hand requires us to build a network topology, but after some negotiations and became a Linux stuff instead. As a Linux user myself I completed the project pretty easily. Now comes, the final project that will be showcased at Expo too. A peak project so to speak. Introducing...

## Chapter 3: Rent N Go

So what's the start of Rent N Go? why car rental out of sudden? why we're not continuing BodyFits from the previous semester? Well, the answers to these questions are actually pretty simple, it's mainly because BodyFits has a lot of issues and bugs. In case I haven't told you in the previous blog, it's a mess. At the start, I spent some minutes trying to get BodyFits to work, and eventually it did though. However, going back to it... uh... nah. Because of that we thought to create something new and decided to go with Rent N Go.

Then what with the Stack? Why do we choose Go and React? is it because our app name is Rent N Go? The answer of course **not**. Rather... 

> As the evening sky painted itself in shades of orange and purple, me and my friends decided to make our way to Badminton Stadium. To play some game.

After many rounds, I randomly get an idea about making a bet with my friends. Since my friends were still motivated and they were kind of lost in what tech stack to learn, I decided to bring up Go into tables. So we begin a match where when they lose against me they need to learn Go. And that's how Go gets selected for the tech stack to the Backend.

We spent our time learning Go using this tutorial [Dasar Pemograman Golang[ID]](https://dasarpemrogramangolang.novalagung.com/1-berkenalan-dengan-golang.html) a good read if you are new to Go too. Then for the framework we choose [GoFiber](https://gofiber.io/). Why? because it's the fastest. The website even promotes itself as the fast web framework, they even provide the benchmark. Last, for the ORM we're going with [GORM](https://gorm.io/) simply because it's the most popular ORM for GO. As for frontend, we stick to React as most of my friends are more comfortable using it.

So we begin learning Go and discussing the features. Car Rental is a basic things but a car rental that provides driver and tour? Well we have fewer in markets for those service (it does exist). So eventually, we came up with Car Rental, Car + Driver, Tour (which also allow you to book the driver and car). Notice each of the feature actually built upon each other, and only Car feature that can run individually. If represented on chain `Tour -> Driver -> Car`.

## Chapter 4: Structuring the Project

Okay, we have decided the features and the tech stacks to use. But how to structure the project? None of us were experienced in building Backend using Go. In fact this project become our first time in building Backend on Go and our first Go Project as well (Damn, should have started with Todo...). And simply enough, because of that we came up with quite some problem. First we need to know how "Go" modules works.

In Go every file have a namespace, these are represented by the `package` keyword whereas `package main` represents the root of the project. So usually Go Devs would build an entry point let's say `main.go` and put their `package main` there. Then afterwards, they can put the rest of their code in the folder. For instance folder utils will have `package utils` and so on.

Now in Go, every declaration are bound to its package unless the name starts with capital letter first. That is the convention Go uses to mark as public and export the declaration. Think of it similarly to `public` and `export` in other language. In Go both of these declarations are combined by putting Capital First on declaration name. 

```go[file.go]
func Test() {} // exported
var Test = "" // exported
type Test struct {} // exported

type User struct {
  Name string // visibility public
}
```

and each of these declaration they're usually bound to it's package. So in `package main` you can't have two function with same name even though you define them in different file. Now because of that in order to get such behaviour these directory structure were used in our project:

- controller
  - controllerName
    - controller.go
    - request.go
    - *_service.go

You can see the rest on [Rent N Go Backend](https://github.com/albetnov/Rent-N-Go-Backend). It works well, really. However the problem is within the root we defined many Go Package which are not really some problem it's just in order to run it, we need to use `go run *.go`. It compiles fun though. However some of these file are actually not related to the project itself. CLI Helper such as `migrate` to migrate the database and `seeder` to fill fake data to the database doens't have to be compiled as these are only used in development environment.

Then we also introduced `kernel.go` it's more like a Bootstrapper so might be better to name it `bootstrap.go` instead. In kernel, we perform all the registrations required for validation, before, after hooks, and etc. We also introduced Hooks there to be executed before any handler passed to Fiber and then afterHook to executes after Fiber done handling. So in before hook we can bootstrap stuff such as Database and etc. Kernel also bootstraps and apply any configuration the app have.

The `server.go` focused on building the Fiber App. Putting any middleware we want to use, mapping routes, and registering and running the hooks. And then, because we're using a minimal framework such as Fiber we need to manually setup our own migrator and seeder for the database and resulting in command such as: `go run *.go migrate` and `go run *.go seed`. Not the best approach but work.

We also connects GORM and GORM Gen with the database (MySQL), setting up Go Playground to validate our payload, integrating fiber locals, and much more. Some of shared logic are placed within `Repositories` these folder contains Database Operations Layer for the app, and then the rest of the structure using MVC model.

> To be continued in: [Rent N Go (Part 2)](https://albetnv.me/blogs/rent-n-go-part-2)