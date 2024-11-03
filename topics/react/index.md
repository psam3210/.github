<br><br>
<figure align="center">
  <img src="macintosh.jpg">
  <figcaption>Teardown of a Macintosh, Things Come Apart, Todd McLellan.</figcaption>
</figure>
<br><br>

## React, Angular, Vue, Svelte, and more

In the past decade, JavaScript frameworks for building user interfaces have exploded. They build on top of standard HTML, CSS, and JavaScript and provide declarative and component-based programming models that help you efficiently develop user interfaces.

React is a [single page application](https://en.wikipedia.org/wiki/Single-page_application) framework, much like Vue, Svelte, or Angular. SPAs work by loading all the application code and logic into the browser when you load the page. Then, based on your interaction, it will conditionally render or change the contents presented on the page. Entire websites are built using these frameworks, most famously Facebook.

## Build Pipelines

We’ve so far worked with native JS files. However, React introduces additional syntax and keywords which mean browsers cannot natively read those files. This means, we will have to use a build pipeline.

A build pipeline consists of:

1. **A package manager** such as npm, yarn, pnpm which allows us to install, update, and work with third party packages.
2. **A bundler** such as webpack, Vite, which compiles all of our modular code snippets and packages into something that browsers can actually read.

For this example, we’ll be using [Vite](https://vite.dev/).

## Using Vite

We’ll be using Vite to set up our React project:

1. `npm create vite@latest`
2. Name your project
3. Select `React` and `JavaScript`
4. `cd` into that folder
5. `npm install` will install any necessary packages
6. `npm run dev` will run Vite’s built in dev script


## Anatomy of a Vite Project

```
vite-project/
├── node_modules/
├── public/
│   └── vite.svg
├── src/
│   ├─ assets/
│   │  └── react.svg
│   ├─ App.css
│   ├─ App.jsx
│   ├─ index.css
│   ├─ main.jsx
├── .gitignore
├── eslint.config.js
├── index.html
├── package-lock.json
├── package.json
├── README.md
└── vite.config.js
```

- `node_modules/` contains any 3rd party dependencies we might have used
- `public/` assets that don’t have to be processed by Vite
- `src/` the source code for our application
- `.gitignore` a standard file that tells git to ignore certain things
- `eslint.config.js` a configuration file for keeping our JS neat and clean
- `index.html` a very basic HTML file that we’ll hook into with React
- `package-lock.json` a generated file that tells Vite which package versions we’re using
- `package.json` a configuration of what our app’s dependencies and scripts are
- `README.md` includes instructions on how to use this repository
- `vite.config.js` a configuration for Vite

## Publishing our Project

Let’s make sure this project will build properly before we do anything else. We’ll create a new GitHub repository and modify our settings based on this [documentation](https://vite.dev/guide/static-deploy.html#github-pages).

Set the `base` variable in the `vite.config.js` configuration to your repository:

```js
export default defineConfig({
  plugins: [react()],
  base: '/react-vite-demo/'
})
```

We’ll also have to configure our code to run in GitHub Pages. Create a new file at `.github/workflows/build.yml` and add the following code:

```yml
# Simple workflow for deploying static content to GitHub Pages
name: Deploy static content to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ['main']

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets the GITHUB_TOKEN permissions to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: 'pages'
  cancel-in-progress: true

jobs:
  # Single deploy job since we're just deploying
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Set up Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
      - name: Install dependencies
        run: npm ci
      - name: Build
        run: npm run build
      - name: Setup Pages
        uses: actions/configure-pages@v4
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          # Upload dist folder
          path: './dist'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

This should kick off a GitHub pages build and we can access it online now!


## Components

Let’s go back and take a closer look at our `src/` folder. You’ll notice that all the files heavily rely on the `import` and `export` framework we previously discussed. 

The bread and butter of any  SPA based web app is components. A [component](https://react.dev/reference/react/Component) allows us to divide our interface elements into independent and reusable parts. Just like we want to modularize our JavaScripts, HTML, and CSS, SPAs let us modularize our interfaces. An app is created by organizing a collection of nested components. 

A React component is **self contained**. It will contain the HTML required to show it and the JavaScript required to make it interactive. 

In our example, `App.jsx` is our App component, and there is an analogous `App.css` that provides the CSS required to render it correctly. This makes components very portable!

React makes use of the existing custom component syntax that exists within HTML, but implements its own behavior. The goal is to represent an interface in a clean and self-contained way. 

## State

React uses a unidirectional data flow model. That means that data will always flow in one direction. In `App.jsx`, we see a function `useState()` which sets the state of a variable `count`. We also have a function as well called `setCount` which lets us update the value of `count`. 

Anytime we update this variable using `setCount`, the entire state of the app will update and then the component will rerender itself. That is how we can declaratively use `{count}` in our JSX (HTML + React syntax) which will always update anytime the state is updated.

## Making a To Do list

Let’s first create a new folder, called `components/` inside of our `src/` folder.

We’ll first start by making our `ToDo.jsx` file. What does this need to do?

1. Keep track of a list of To Dos with [`useState()`](https://react.dev/reference/react/useState)
2. Take an input and add to the list of To Dos
3. Store our list of To Dos in `localStorage` with [`useEffect()`](https://react.dev/reference/react/useEffect)
4. Show the list of To Dos

We’ll also make a `TodoItem.jsx` file which represents a single To Do item. This will need to:

1. Show a To Do item
2. Mark if the item is done or not, and modify its appearance if so
3. Allow us to remove the item
