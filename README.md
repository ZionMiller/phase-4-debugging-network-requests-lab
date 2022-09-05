# Putting it All Together: Client-Server Communication

## Learning Goals

- Understand how to communicate between client and server using fetch, and how
  the server will process the request based on the URL, HTTP verb, and request
  body
- Debug common problems that occur as part of the request-response cycle

## Introduction

Just like the last lesson, we've got code for a React frontend and Rails API
backend set up. This time though, it's up to you to use your debugging skills to
find and fix the errors!

To get the backend set up, run:

```console
$ bundle install
$ rails db:migrate db:seed
$ rails s
```

Then, in a new terminal, run the frontend:

```console
$ npm install --prefix client
$ npm start --prefix client
```

Confirm both applications are up and running by visiting
[`localhost:4000`](http://localhost:4000) and viewing the list of toys in your
React application.

## Deliverables

In this application, we have the following features:

- Display a list of all the toys
- Add a new toy when the toy form is submitted
- Update the number of likes for a toy
- Donate a toy to Goodwill (and delete it from our database)

The code is in place for all these features on our frontend, but there are some
problems with our API! We're able to display all the toys, but the other three
features are broken.

Use your debugging tools to find and fix these issues.

There are no tests for this lesson, so you'll need to do your debugging in the
browser and using the Rails server logs and `byebug`.

**Note**: You shouldn't need to modify any of the React code to get the
application working. You should only need to change the code for the Rails API.

As you work on debugging these issues, use the space in this README file to take
notes about your debugging process. Being a strong debugger is all about
developing a process, and it's helpful to document your steps as part of
developing your own process.

## Your Notes Here

- Add a new toy when the toy form is submitted

  - How I debugged:
  * I fixed this last
  1) I first navigated to the form in browser, input valid data and made a request
  2) From here I recieved a 500 (Internal Server Error)
  3) Rembering that the instructions informed us that our client won't need to be touched, I think this error will be in our controller since the route appears to exist
  4) I navigated to app>controllers>toys_controller.rb and looked for the create action which is a POST
  5) Here I discovered our toy variable was initialized with a plural Toys.create(toy_params) when it should be Toy.create(toy_params)
  6) I removed the s and tested in our browser
  7) Initially I recieved a 404 due to an invalid image, I copied a prexisiting image below and submitted with a funny name to make sure I could spot it
  8) Thankfully now everything is working, I tested all actions on a couple cards and moved on!

- Update the number of likes for a toy

  - How I debugged:
  1) Checked browser, fired off requests and went to console to observe errors
  2) Recieved "Uncaught (in promise) SyntaxError: Unexpected end of JSON input"
  3) According to the instructions, we don't need to edit our React which pointed me instead to our controller
  4) I assumed this because this errors means we aren't rendering json somewhere in our response. Since client doesn't need to be touched, this must mean our controller is missing something
  5) Likes are a PATCH so I looked for the update action and sure enough, the action was missing a render

- Donate a toy to Goodwill (and delete it from our database)

  - How I debugged:
  1) *I noticed our destroy action was also missing a render json, but then remembered destroy doesn't need this so I:
  2) I navigated back to our browser to observe the error 
  3) This returned a "404 not found" which is a hint that this route doesn't exist yet
  4) Sure enough config>routes.rb reveales we are only permitting index, create, update, so I added :destroy action
  5) tested on two cards to confirm and observed response in network in the console
