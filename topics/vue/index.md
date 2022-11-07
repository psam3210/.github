<br><br>
<figure align="center">
  <img src="components.png">
  <figcaption><a href="https://vuejs.org/guide/essentials/component-basics.html">Destructuring a web page into components.</a></figcaption>
</figure>
<br><br>

## What is Vue?

Vue is a JavaScript framework for building user interfaces. It builds on top of standard HTML, CSS, and JavaScript and provides a declarative and component-based programming model that helps you efficiently develop user interfaces, be they simple or complex.

Vue is a [single page application](https://en.wikipedia.org/wiki/Single-page_application) framework, much like React, Svelte, or Angular. SPAs work by loading all the application code and logic into the browser when you load the page. Then, based on your interaction, it will conditionally render or change the contents presented on the page. Entire websites are built using these frameworks, most famously Facebook.

## Let’s get started!

Prior to class, install [Node](https://nodejs.org/en/). If you’ve been using running your assignment tests locally, you’ve already done this.

<br><br>
<figure align="center">
  <img src="install.png">
  <figcaption>Installing Vue</figcaption>
</figure>
<br><br>

Now, create a Vue app in your code directory, and follow the prompts. For now, select no for everything except for the first prompt.

```sh
npm create vue@3
```

After you run through the prompts, follow the commands you were given:

```sh
cd vue-project
npm install
npm run dev
```

This will first change your directory to the directory with your Vue application in it. 

## Deploy to GitHub pages

in `.github/workflows/node.js.yml`
```yml
# This workflow will do a clean installation of node dependencies, cache/restore them, build the source code and run tests across different versions of node
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-nodejs

name: Node.js CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16.x]
        # See supported Node.js release schedule at https://nodejs.org/en/about/releases/

    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    - run: npm ci
    - run: npm run build --if-present
    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
```
[Thanks](https://blog.codedbyjordan.com/deploying-a-vite-app-with-github-actions)

https://vitejs.dev/guide/static-deploy.html

## More resources

- [Vue Tutorial](https://vuejs.org/tutorial/#step-1)
- [Vue Documentation](https://vuejs.org/guide/essentials/application.html)
- [Vite](https://vitejs.dev/)