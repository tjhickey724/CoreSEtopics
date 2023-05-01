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

Today we'll show how to create a nextjs app from scratch
* How to add pages to the app and how to Link to them from other pages
* How to create a general Layout that applies to every page
* How to add bootstrap styling to all pages
* How to interact with the server using the api/ routes with handler functions

We will use the [nextjs documentation](https://nextjs.org/docs/getting-started) as a guide.

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

# replace the index.js file with our own
Let's remove the default index.js file which is a nextjs ad, and replace it with the following.
The Head tag allows us to add elements to the <head> element of the webpage.
The Link tag allows us to link t other pages, we will write the "pages/test.js" page soon.
``` javascript
// pages/index.js
  
import Head from 'next/head'
import Link from 'next/link';

export default function Home() {
  return (
    <>
      <Head>
        <title>CS103a NextJS Demo</title>
        <meta name="description" content="Generated by create next app" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <link rel="icon" href="/favicon.ico" />
      </Head>
      <main>
        <h1>Our First NextJS Demo!</h1>
        <Link href="/test">Visit the test page</Link>
      </main>
    </>
  )
}
```
# adding pages to the app and Linking
Simply create a file newpage.py in the pages folder
and access it via the route /newpage
Let's try that with an MLA exercise...

# adding bootstrap to the nextjs app
Step 1 is to install bootstrap in the node_modules
``` bash
npm install bootstrap
```
Next modify the _app.js file to import the bootstrap css and 
the bootstrap javascript (but this has to be done with use_effect
as it must be run in the browser not in the server...
``` javascript
import React,{useEffect} from 'react';

import 'bootstrap/dist/css/bootstrap.css';
//import '@/styles/globals.css';

export default function App({ Component, pageProps }) {
  useEffect(() => {
    require("bootstrap/dist/js/bootstrap.bundle.min.js");
  }, []);
  return (
      <Component {...pageProps} />
  ) 
}

```

## Creating a general Layout
First create a file layout.js  as follows:
``` javascript
// pages/layout.js

export default function Layout({ children }) {
  return (
    <>
      <h1>layout</h1>
      <main>{children}</main>
    </>
  )
}
```
the ```{children}``` prop holds the JSX children of any JSX element.
This is different from the attributes of the element, it is everything between the open and close tags...

Then we modify the _app.js to use this Layout
```
// pages/_app.js
import React,{useEffect} from 'react';
import Layout from './layout';
import 'bootstrap/dist/css/bootstrap.css';
//import '@/styles/globals.css';

export default function App({ Component, pageProps }) {
  useEffect(() => {
    require("bootstrap/dist/js/bootstrap.bundle.min.js");
  }, []);
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  ) 
}

```
Finally, let's make this nicer by adding a [navbar from Bootstrap](https://getbootstrap.com/docs/5.3/components/navbar/#nav)
``` javascript
\\ pages/navbar.js

import React from 'react';

export default function navbar(){
  return (
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
        <div class="container-fluid">
            <a class="navbar-brand" href="#">Navbar</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item">
                <a class="nav-link active" aria-current="page" href="#">Home</a>
                </li>
                <li class="nav-item">
                <a class="nav-link" href="#">Features</a>
                </li>
                <li class="nav-item">
                <a class="nav-link" href="#">Pricing</a>
                </li>
                <li class="nav-item">
                <a class="nav-link disabled">Disabled</a>
                </li>
            </ul>
            </div>
        </div>
        </nav>
  )
}
```
and add the Navbar to the pages/layout.js page
``` javascript
// pages/layout.js

import Navbar from './navbar'

export default function Layout({ children }) {
  return (
    <>
      <Navbar />
      <main>{children}</main>
    </>
  )
}
```
## customizing the navbar
Let's now customize the navbar by using our own Links
``` javascript
import React from 'react';
import Link from 'next/link';

export default function navbar(){
  return (
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
        <div class="container-fluid">
            <a class="navbar-brand" href="#">Navbar</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item">
                <Link class="nav-link active" aria-current="page" href="/">Home</Link>
                </li>
                <li class="nav-item">
                <a class="nav-link" href="/test">Test via a-href</a>
                </li>
                <li class="nav-item">
                    <Link className="nav-link" 
                          href="/test"> Test via Link</Link>
                
                </li>
                <li class="nav-item">
                <a class="nav-link disabled">Disabled</a>
                </li>
            </ul>
            </div>
        </div>
        </nav>
  )
}
```

