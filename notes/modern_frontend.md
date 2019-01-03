* [Webpack](#webpack)
* [React](#react)

# Webpack
[awesome webpack](https://github.com/webpack-contrib/awesome-webpack#webpack-plugins)

## Better Understand

* loaders vs plugins
* what goes in the babelrc file, and why

## Environment Variables

* webpack does not allow ENV vars to slip into the app, unless they're explicitly defined, in the configs
* add webpack.DefinePlugin() entry into plugins: section, to define NODE_ENV for the app. Important to do this, because NODE_ENV is the convention for other packages as well (in node_modules), which will help reduce output file sizes as well
```
plugins: [
    new webpack.DefinePlugin({
      "process.env": {
        NODE_ENV: JSON.stringify("development")
      }
    })
  ]
```


## Configuration

* use webpack-merge to merge config files, reducing redundancies. Shown in the Udemy SSR course
* [comprehensive options overview](https://webpack.js.org/configuration/#options)
* [config file generators](https://webpack.js.org/configuration/#configuration-file-generators)

### Entry

* In Webpack 4, "Do not create a entry for vendors or other stuff which is not the starting point of execution." Use optimization.splitChunks instead
* "As a rule of thumb: for each HTML document use exactly one entry point."
* add polyfills (babel) before any project js files

### Output
* the output file and directory, many options here

### Mode
* [table outlining effects of devel, prod](https://webpack.js.org/concepts/mode/#mode-development)

### Loaders
* QSTN: They allow for options, what are they?

### Plugins
* serve the purpose of doing anything Loaders cannot do

### The Manifest
* Becomes important to understand once [browser caching is implemented](https://webpack.js.org/concepts/manifest/#the-problem)

### devServer
* used to configure webpack-dev-server

### devtool
* for generating source maps

### target
* determine which environment, like web, node, and webworker among others

### watch
* watch for file changes. In webpack-dev-server and webpack-dev-middleware watch mode is enabled by default.

### externals
* provides a way of excluding dependencies from the output bundles

### stats
* The stats option lets you precisely control what bundle information gets displayed. Many options

### More config notes

* minification and uglifying plugins
    * babel-minify, babel-minify-webpack-plugin
    * uglifyjs-webpack-plugin
    * uglify is more established, babel is up and coming
* compression
    * **Note**, that if the file sizes are small to being with, the compressors may skip them. I had this happen with gzip
    * gzip - understood by all browsers
    * brotoli - further compression, maybe not as well supported
    * express server will server normal files if compressed files don't exist, like in development
* analyser plugin
    * webpack-bundle-analyzer - Plugin to visualize size of webpack output files with an interactive zoomable treemap.
    * with dev server running, it automatically opens a page on port 8888 with stats
* Hot reloading
    * webpack-hot-middleware for client. Adds hot reloading when not using webpack-dev-server
    * webpack-hot-server-middleware for server
    * can use nodemon to monitor changes in server files (like express). Files not covered with webpack server config. nodemon may not be necessary though

# React

## Learning and Resources

* https://reactjs.org/community/external-resources.html
* [cra-with-ssr extensive resource](https://github.com/zhirzh/cra-with-ssr/tree/master/client#code-splitting)

## Frameworks

### create-react-app
* version 2 came out 10/18
    * babel 7, webpack 4, Sass, and other improvements

### Next.js
* Server-rendered by default
* Automatic code splitting for faster page loads
* Simple client-side routing (page based)
* Webpack-based dev environment which supports (HMR)
* Able to implement with Express or any other Node.js HTTP server
* Customizable with your own Babel and Webpack configurations

### Razzle
Universal JavaScript applications are tough to setup. Either you buy into a framework like Next.js or react-server, fork a boilerplate, or set things up yourself.

Razzle is a tool that abstracts all complex configuration needed for SSR into a single dependency--giving you the awesome developer experience of create-react-app, but then leaving the rest of your app's architectural decisions about frameworks, routing, and data fetching up to you. With this approach, Razzle not only works with React, but also Reason, Elm, Vue, Angular, and most importantly......whatever comes next.

## CSS Solutions

* article on the types: https://alligator.io/react/react-css/ )
    * traditional (w/ sass, if desired)
    * javascript stylesheets
    * modular stylesheets
    * stylized components ( emotion | glamorous | styled-components ). There may be corresponding React packages for these as well (react-emotion)

      What’s a problem with “styled components”?
        * Sometimes hard to overwrite a CSS framework you’re already using.
        * If you’re new into styled components, you’ll likely connect application logic directly with styles. That’s incorrect.

        Some recommendations when dealing with styled-components:

        * Don’t make a styled component for every component in your app. Use styled-components only for reusable/generic components.
        * HOCs could save you some time.
        * Don’t mix logic props and UI props.

## Optimizations

* create-react-app
    * to gzip, add entry to npm scripts
        * "build-gzip": "find .\/build \\( -name '*.css' -o -name '*.js' \\) -exec gzip --verbose --keep {} \\;"
        * [from here](https://askubuntu.com/questions/496432/gzip-all-files-with-specific-extensions)

## Data Handling

* [How to fetch data in React](https://www.robinwieruch.de/react-fetching-data)

# Debugging

* Node (server-side code)
    * add the --inspect flag to node or nodemon commands
    * in Chrome tools, look for Node logo, and open (DevTools)
        * **Connection Tab**:  if connection isn't automatic, add connection with the port specified in the console of npm run dev
        * **Console Tab**:  should mirror the console of the npm run dev
        * **Sources Tab**:  Node Tab - project code contained under file:// -> project path
* Client-side code using webpack
    * Chrome tools -> Sources Tab -> top -> localhost -> webpack:// -> . (directory)
* React
    * Chrome extension: React Developer Tools. Need to configure for igcognito mode, in chrome://extensions