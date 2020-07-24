# devNursery
### by Alex Merced of AlexMercedCoder.com

## Project Summary

devNursery is a resource for developers to learn more, meet other devs and write content.

#### User Stories

- User can find video tutorials on web technologies

- User can join slack community to meet other devs

- Users can sign up to be able to write blog posts

- Users will get a link to a page to share their blog with the world

- users have a private todo list that keeps track of todos

## Architecture

This app is made of several small apps or "microservices"

### App 1 - The api (AWS Lambda, Express, Mongo)
##### https://github.com/AlexMercedCoder/devNurseyBlogAPI
(This portion of the app has been retired)

The API is written with Node/Express but deployed via AWS Lambda using the Serverless framework. I eventually hit an issue with CORS but found a library that provided some middleware that gets eliminates the issue. I later ran into an issue that since Lambda is event driven and not a persistent server that I couldn't use session to store user login, which I found a front-end workaround for.

### App 2 - Front page of devNursery.com (React/Typescript)
##### https://github.com/AlexMercedCoder/devNurseryFrontReact

The homepage application for devNursery.com was created using React/Typescript which was completed fairly quickly. Typescript added some extra tediousness to React as the Typescript template set the Typescript compiler to a strict setting where I had to make sure every prop defined and typed before I could pass it. While tedious, I can see how this can make errors quicker to catch if the code is refactored later. Deployed to netlify.

### App 3 - Blogging Application (Svelte)
##### https://github.com/AlexMercedCoder/devNurserySvelteBlogApp

The blogging application is being built with Svelte interacting with the API from App 1. The app allows users to create a user account, login and create posts. To minimize api calls it stores a copy of the users posts in localstorage till they logout, this makes the UX faster and more seamless and the potential AWS expenses for invoking lambda functions minimal. Will be refactoring this app to just a be a general members area for any features I create going forward with the new express API.

### App 4 - Public Blog for users (VUE/NUXT)
##### https://github.com/AlexMercedCoder/devNurseryPublicPageVue
(This portion of the app has been retired)

Will create a separate full stack app using Vue and its backend framework, Nuxt. The purpose of this app is to reduce the load on the API and to be able to provide all users with a public blog page. I had to create a seperate backend so I could use URL params in rendering my users blog page. I could have done this React/Next, Svelte/Sapper or Angular/Nest but I wanted some more Vue exposure so this sound like an easy to get that exposure with. Essentially each user will get a url like blogger.devnursery.com/username that will generate a page with their blog posts that they can share. I originally wanted to create separate database access through Nuxt but implementing mongoose was a little trickier than expected so currently it makes calls to the Lambda function to grab posts. I will likely try to make a separate API purely for the use of this application using GO buffalo to minimize the load on AWS for user blog requests.

### App 5 - Todo list api (GO/BUFFALO)
##### https://github.com/AlexMercedCoder/GoBuffaloApi
(This portion of the app has been retired)

So the next application I tackled was creating an API for todo lists using the GO programming language, the Buffalo framework, and a PostgreSQL database. The buffalo framework really did a lot of the heavy lifting with its resource feature which will generate a bundle of routes. I had to create one additional route for getting a specific users todos, then I spent way more time figuring out how to deploy it to heroku which turned out not to be so bad with the Buffalo Heroku plugin so I was able to complete this app in a day. After I had to take a few days off the project to do a deep dive into GraphQL and Apollo/Apollo Server for a potential role working with GraphQL apis.

### App 6 - Front-End for Todo Application (Angular 8)
##### https://github.com/AlexMercedCoder/devNurserytodoAngular
(This portion of the app has been retired)


I now had to create an interface for the todo list application I chose to use angular to create it. I made us of many of the most common Angular features (ngif, ngfor, one way bind, two way binding) and then ran into a few logistical issues regarding authentication.

My first plan was to pull the login info from localstorage but even different subdomains have distinct localstorage. Then I tried checking the session with the blog api but forgot since it is serverless there is no persistent session. I ended up nesting the angular app build inside the svelte app build for the blogging app so they'd sit on the same subdomain and have access to the same localstorage.

### App 7 - GraphQL Server to haromonize Postgres and Mongo
##### https://github.com/AlexMercedCoder/devnGraphQLApi
(This portion of the app has been retired)

Creating a GraphQL server with Apollo Server to deliver blogs from the mongo database and todos from the postgres database from one unified API

### App 8 - StencilJS Summary Page
##### https://github.com/AlexMercedCoder/devNurserySummaryPage
(This portion of the app has been retired)

To test out the GraphQL API, a summary page created with StencilJS was added to the member dashboard that summarizes the users content for the user.

### App 9 - ExpressJS API
##### https://github.com/AlexMercedCoder/devNurseryAuthApi

To replace many of the backend API I had created prior I'm unifying the backend of devNursery into this express app to make features easier to maintain instead of across several disparate apps.
