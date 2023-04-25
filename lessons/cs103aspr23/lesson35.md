# Review ToDoList app
We go over the final version of the ToDoList app and show
* how to define components in their own files and import them into an app
* how to create multiple pages and switch between them with menu items

# Overview of Nextjs
We next show how to create a nextjs app. The main advantage of using nextjs over a pure React App are
that nextjs make it easier to
* create multipage apps and link between them
* interact with the server
* deploy the app to a server in the cloud

# Creating a Nextjs app from scratch
We will work through the steps of creating a nextjs app from scratch and explain all parts of the app as we go

## First step, using create-next-app
We begin by cd'ing to a place that you want to develop your app
and then invoking the following command which will guide you through
a series of yes/no questions (which you answer using the arrow keys to select,
and the return key to accept).

It will first ask you for the name of the project though and will create a folder
with that name containing the nextjs code.

``` bash
npx create-next-app@latest
```
which results in this interaction
``` bash
(base) tim@MacBook-Pro-3 ~ % cd Desktop 
(base) tim@MacBook-Pro-3 Desktop % npx create-next-app@latest
✔ What is your project named? … **next_demo_1**
✔ Would you like to use TypeScript with this project? …** No **/ Yes
✔ Would you like to use ESLint with this project? … No / **Yes**
✔ Would you like to use Tailwind CSS with this project? …** No** / Yes
✔ Would you like to use `src/` directory with this project? … No /** Yes**
✔ Would you like to use experimental `app/` directory with this project? …** No** / Yes
✔ What import alias would you like configured? … @/*
Creating a new Next.js app in /Users/tim/Desktop/next_demo_1.

Using npm.

Initializing project with template: default 


Installing dependencies:
- react
- react-dom
- next
- eslint
- eslint-config-next


added 267 packages, and audited 268 packages in 15s

105 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
Initialized a git repository.

Success! Created next_demo_1 at /Users/tim/Desktop/next_demo_1
```
