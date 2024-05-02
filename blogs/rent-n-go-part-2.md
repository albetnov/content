---
title: Rent N Go (Part 2)
description: The continuation of the previous Rent N Go (Part 1) Blog. Rent N Go. The second iteration of our Expo App. Rent-N-Go offers a variety of services for car rental. It features Car Rental, Driver, and Tour. This blog is about the creation of Rent-N-Go, why Car Rental, how we build it, why it uses Go and many more.
categories: [3]
collections: [2, 3]
created_at: 2024-05-01
updated_at: false
archived: false
---


# Rent N Go (Part 2)

:cover-image{src="/blogs/zepdb6oklm6r8zdtjlvu.jpg" alt="Rent N Go (Part 2)"}

## Quick Summary

Previously in the [Rent N Go Part 1](https://www.albetnv.me/blogs/rent-n-go-part-1) I shared about how the project will be structured and what kind of tech stack is used on the project (Fiber, GORM, React). I also mentioned what project we building at that time. But for now, let's continue our focus with the Rent N Go App.

In case you don't know what Rent N Go is, think of it as a Rental Cars app (Generic). The app features core functionality such as Car Rental of course with additional two features Driver, and Tour. Allowing you as a customer to rent a car, with a driver, with a tour where the destination has been taken care of by us while still remaining private. The app has 2 roles, a customer which ability I have mentioned earlier, and an Admin who can manage the website entirely (adding/removing cars, tours, drivers, etc).

<!--more-->

## Creating The API

Creating the API in the Fiber App is pretty straightforward. It's similar to [Express API](https://expressjs.com/) all I need is just to configure CORS using their first-party [CORS Middleware](https://docs.gofiber.io/api/middleware/cors) setting it allows origins and headers and good to go. The Fiber API also allows you to Group routes with their middleware see [Grouping Routes](https://docs.gofiber.io/guide/grouping). With that, I can make two route groups being authenticated and non-authenticated (guest mode). The next thing is to implement authentication.

Since our customer-facing app was created in React, you know separate front-end and back-end. We need to create an authentication that is stateless not using any session. Why? because usually Session IDs are stored within cookies. Cookies are sent every time a request is made to the same origin. So if our top-level domain is the same, this won't pose any problem (E.g. api.example.com for backend and example.com for frontend) But to avoid nuisance in the future (although the app is not hosted) we decided to implement stateless authentication instead by using [JWT](https://jwt.io/) which are also quite simple to implement.

Luckily Fiber also has third-party support for JWT under their [JWT Contrib](https://docs.gofiber.io/contrib/jwt/) middleware. The thing is the way you separate authenticated and un-authenticated rules is like this:

```go[routes.go]
app := fiber.New()

// any routes defined here are un-authenticated

app.Use(jwtware.New(jwtware.Config{
  SigningKey: jwtware.SigningKey{Key: []byte("secret")},
}))

// Any routes defined here however need to be authenticated
```

It is the expected behavior, the only problem is this rule applies to all routes including those that are not defined. So for instance, if I had `/abc` and then I accessed `/def` I need to have the token to get the appropriate 404 messages. But, Can a group solve this? earlier we knew that by using a group you can add middleware to the members of the groups. At the time of writing the app, I am not sure how Fiber handles groups today. The way they apply the middleware to grouped routes is by using group path prefix match. Meaning, that assuming you have two groups with the same prefix, you will get the same middleware. Even though the members of the groups have different paths. So, if we have a group with an empty prefix, it's the same as writing `app.Use()` anyway.

```go[routes.go]
group := app.Group("", middlewares)
group.Get("/about", handler) // `middlewares` applied

app.Get("/test", handler) // `middlewares` also applied
```

This behavior though does not seem to exist in ExpressJS and Laravel, I can group routes fine by just using an empty prefix to indicate that I want the middleware to be applied to all the members while not messing with other routes outside the group. And, because of that, We can't group my routes because I will have to add prefixes for these routes. Because of that, I changed the error message of JWT middleware to also indicate perhaps the routes you're trying to access are not defined or exist.

## Creating Validation

Now that our API is done, the next step is to create the validation to validate user input. Fiber makes it easy to do that, they provide a validator package and a dedicated guide on using them. The only caveats are you need to validate on your handler and you need to return a response on error. So every route will then look like this:

```go[controller.go]
func Index(c *fiber.Ctx) {
  // ...

  if !notValid(validator) {
    // return res 400
  }
}
```

Placing every if statement check makes the code look... well... not nice. This is because in Fiber you need to return the response, unlike Gin where you can return the response from any points in your code. However, there's a way to fix this, using middleware! Every route definition will have to contain middleware that checks for validation. If the validation fails, the middleware will return an invalid response, and the `next` function won't be executed, leaving the handler to not run at all. Sort of, intercepted. And with that, we can remove the need for if checks. However how to transfer the validated/cleaned data over to the handler?

Well turns out, the answer is simple. Fiber has Locals (not localization). Locals means a shared storage upon context. With Locals, we can send data from middleware down to the handler. Think of this similar to React's useContext. Then with the Generic feature of Go, we can assert the type! The API looks like the above:

```go[routes.go]
app.Post("/create", utils.InterceptRequest(Rules), handler)
```

Date Validation is also quite tricky, [Go Playground/Validator](https://github.com/go-playground/validator) At the time I am writing the app has no feature on how to validate date in the format of `before`, `after`, or `same`. Therefore, I need to make a custom validator and just use ISO format for the date. So the front-end has to send a date complete with its timestamp as defined in [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) convention.

## Handling Image Upload

When using API, there are multiple ways to Upload Images or Files. If we want to still use JSON, we can encode the images/files to base64 format first. However, doing this requires more computation. As I need to encode in clients and then decode afterward in the server. Two works. Pain. Alternatively, we can use Multipart Form Upload which allows us to upload the image even in binary format. But this will cause inconsistency in API Design where one is using JSON and the other is using Form Data.

A better approach would be creating a separate API that takes Form Data solely just to upload images. This is what I am doing, in exchange though this approach will double the network transport required to submit a form. First is the form data and then the image. 2 Trips of API Calls. Managing the Upload API also can often be hard, for instance, you don't want some naughty user to upload an image where no forms are available, causing unnecessary resource loss in the end because of having photos uploaded but not used. To fix it, we can add a second payload, perhaps an ID to the form. This requires the FORM to have been submitted before the upload image API is called. Presigned Upload API is a good example of this too.

Rent N Go takes the payload approach where an ID is required in the payload to upload the photo since we're only using it in the Profile menu which happens to make all the requests become some sort of like "Edit" because you need to register to have a profile and this register is where the form data is submitted.

Okay, the Image Upload is solved, right? Well not really, we still run into validation issues. Image Upload should only accept *images* not other files. For that, we need to use a library [Mimetype](https://github.com/gabriel-vasile/mimetype) to help us check the mime types of the user-uploaded image to ensure it is an image. Now that the format validation is done, the next thing to do is Size Validation. We don't want our servers to have gigabytes usage of storage because some naughty user decided to upload large images. To do it, we can set Global settings for Fiber to limit the max file upload size and interrupt if the received file starts to grow bigger than the limit.

But then, can we dynamically adjust the max uploaded files based on the API Routes? The answer is yes, but tricky. First of all, we need to set a large max upload size globally and then in the API, we need to receive the entire file before we can validate the size. So assuming I set the max upload to 10MB, and I want route `/image/upload` to have a 5MB size limit. I would have to accept the entire 10 MB file and store it somewhere in the buffer before I check the size and reject the request. So I decided to just put a global limit and not have to deal with dynamic max upload files based on routes.

> I get the answer above thanks to the Fiber Author Response in the Discord community. They're pretty active. The solution above might not be the best. Perhaps, you can use Stream and Context to abort the request. But I haven't explored further.

If you are interested in how mimes validation is done, check the [validator.go](https://github.com/albetnov/Rent-N-Go-Backend/blob/c5b949e641b1c64606d161a4fd3f602dd38088dc/utils/validator.go#L170-L180) utils.

## Implementing The Frontend

For the front end, continuing the previous experience we use React using [Vite](https://vite.dev) template. So not to a framework first, because NextJS requires more learning than just regular [React](https://react.dev). Then for the REST API, we use [Axios](https://axios-http.com/docs/intro). All of the REST APIs will then mapped to it's each file with error handling, repeated (could use axios interceptor for this). As for the UI we continue to use [Chakra UI](https://v2.chakra-ui.com/) thanks to its simplicity and flexibility.

For routing, we use [React Router](https://reactrouter.com/en/main) together with their Loader and Actions. Allowing us to fetch before the page is rendered to the client. Pretty neat! though re-fetching is quite tedious to do. Then we also use Action to handle submission (partial). Then to handle the authentication we will need a global store. We decided to go with Zustand to help us manage global stores more easily. 

### Loading Animation

We have a modal that shows the car moving every time the app loads, we used free assets uploaded in [Lottie Files](https://lottiefiles.com/), and as you expect we use Lottie to render the animation.

### Banner

As for the promo banner you see on the landing page, we used [Swiper](https://swiperjs.com/) for that. With Swiper, we can get a nice carousel and controls around it.

### Changing Title

Since the app is SPA, we need some way to change the title. For that [React Helmet](https://www.npmjs.com/package/react-helmet) is used to change the title on the fly. However, it didn't really affect SEO that much.

### Creating the Order Wizard

Creating an Order Wizard is quite a pain. In url `/order` the view can change based on the last state of the user interaction with their order. This basically means for every step the user has done we need to keep track of it. So we used Zustand and localStorage to initialize the default value. For every change in step, we will trigger Zustand action to save to both global store and localStorage, allowing us to persist the step and data the user has filled in. Temporarily in localStorage though. So, if the user opened their cart in a different browser. They will keep reset from the first flow.

Each of the flows is implemented as a separate component where JSX Conditionals take place to render them accordingly with the current global state position. Afterward, we also want to fake the callback mechanism of the Payment Gateway while having no actual payment gateway implemented. To do that, we create a fake UUID and store it within the localStorage of the user. Then, we will redirect them to the `/order/process/${uuid}` page where it takes UUID as the parameter. Of course, the check literally just:

```ts[ProcessOrder.tsx]
if (localStorage.getItem(key)) {
  return localStorage.getItem(key) === parameter[0]
}
```

But hey, we kind of replicate it. If the localStorage does not exist the order won't proceed! Really though the workaround of this fake order process is just adding the key manually to your localStorage, then accessing the URL with the parameter of the value of the localStorage you just added earlier.

### Implementing Dropzone

In the profile, you can put your driver's license and your ID there. The UI however requires you to have a drag-and-drop interface and also a text with a link to manually pick the file. In order to implement this I use [React Dropzone](https://react-dropzone.js.org/) library. Which have awesome features out of the box and allow me to quickly finish the design.

Alright. That's all about the front end!

> To be continued in [Rent N Go Part 3](https://albetnv.me/blogs/rent-n-go-part-3)
