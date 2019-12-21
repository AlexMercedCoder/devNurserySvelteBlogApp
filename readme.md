# devNursery
### by Alex Merced of AlexMercedCoder.com

## Project Summary

devNursery is a resource for developers to learn more, meet other devs and write content.

#### User Stories

- User can find video tutorials on web technologies

- User can join slack community to meet other devs

- Users can sign up to be able to write blog posts

- Users will get a link to a page to share their blog with the world

## Architecture

This app is made of several small apps or "microservices"

### App 1 - The api

The API is written with Node/Express but deployed via AWS Lambda using the Serverless framework. I eventually hit an issue with CORS but found a library that provided some middleware that gets eliminates the issue. I later ran into an issue that since Lambda is event driven and not a persistent server that I couldn't use session to store user login, which I found a front-end workaround for.

### App 2 - Front page of devNursery.com

The homepage application for devNursery.com was created using React/Typescript which was completed fairly quickly. Typescript added some extra tediousness to React as the Typescript template set the Typescript compiler to a strict setting where I had to make sure every prop defined and typed before I could pass it. While tedious, I can see how this can make errors quicker to catch if the code is refactored later. Deployed to netlify.

### App 3 - Blogging Application

The blogging application is being built with Svelte interacting with the API from App 1. Will likely be deployed to netlify.

### App 4 - Public Blog for users

Will create a separate full stack app using Vue and its backend framework, Nuxt. The purpose of this app is to reduce the load on the API and to be able to provide all users with a public blog page. I had to create a seperate backend so I could use URL params in rendering my users blog page. I could have done this React/Next, Svelte/Sapper or Angular/Nest but I wanted some more Vue exposure so this sound like an easy to get that exposure with. Essentially each user will get a url like blogger.devnursery.com/username that will generate a page with their blog posts that they can share. Since the back and front will be intertwined, there will be no need for API called as the Nuxt backend will directly pull the data from database and render a VUE page for the user. Likely deployed to Heroku.
