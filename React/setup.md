# [Steps to set up a React application by `create-react-app`](https://github.com/facebook/create-react-app)

## Installing create-react-app

With NodeJS/NPM installed on your machine,
```
npm install -g create-react-app
```

It's recommended to install it **globally** so that it can be used at any location and for creating multiple React projects.

## Create a New React App

```
yarn create react-app my-app
cd my-app
```

The folder looks like this:

```
my-app
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    └── serviceWorker.js
```

These are the basic required files for our app.

`create-react-app` uses `Babel` and `Webpack` behind the scenes.

## What does package.json look like ?

```json
{
  "name": "my-react-tutorial-app",
  "version": "0.1.0",  
  "private": true,
  "dependencies": {
    "react": "^16.5.2",
    "react-dom": "^16.5.2",
    "react-scripts": "1.0.7"
  },
}
```

add `scripts` for our react application:

```json
{
  "name": "my-react-tutorial-app",
  "version": "0.1.0",  
  "private": true,
  "dependencies": {
    "react": "^16.5.2",
    "react-dom": "^16.5.2",
    "react-scripts": "1.0.7"
  },
  "devDependencies": {
    "react-scripts": "1.0.7"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

Here, we have the following attributes:

- **``name``**
  - Contains the app name which we passed to create-react-app.
- **``version``**
  - Displays the current version.
- **``dependencies``**
  - Displays all the required modules/versions required for our app. By default, npm would install the most recent major version.
- **``devDependencies``**
  - Displays all the modules/versions for testing the app in a development environment.
- **``scripts``**
  - Has aliases that can be used to access react-scripts commands in an efficient manner.
  - For example, running ``npm build`` in the command line will actually run "``react-scripts build``" behind the scenes.

### Node Modules

If we have a look inside node_modules, we'll see that it **contains all the "dependencies" and "devDependencies" required** by our React app.

These are as specified or seen in our ``package.json`` file. If we just run the ``ls -l`` command, we'll see almost 700 - 800 sub-directories.

This directory gets added to ``.gitignore``, so it does not really get uploaded/published as such. Also, once we **minify** or **compress** our code for production, our basic tutorial app would not be more than 100 KB.

### Public Directory

This folder has **all the assets which will be served directly without requiring any kind of additional processing by Webpack**. 

``index.html`` is the **entry point** for our tutorial project/app. We can also see a ``favicon`` (or header icon) plus ``manifest.json``.

This ``manifest`` file provides **configuration** for our web application and shows how the application will behave once it gets added to any mobile-user’s home screen.

## Build and Run Our App

```
cd my-react-tutorial-app
```

=> To **build** this app - for production to the **build** directory:

```
yarn build
```

The **"build" folder** would contain all the **static** files which can be directly used on any web server.

Also, the build command **transpiles our source code** into code which the browser can understand. It uses ``Babel`` for this and files are optimized for best performance. All of our ``JS`` files are **bundled** into **a single minified file** and even HTML/CSS code is minified to significantly reduce the download times on the client's browser.

=> To **start** this app - to run in development mode:

```
yarn start
```

It opens ``http://localhost:3000`` automatically to preview our app live.

The page will automatically **reload** whenever it detects any code change in the souce files. Warnings and errors can also be seen in the console.

Internally, ``npm start`` uses ``webpack dev server`` to start a dev server so that we can communicate with the same.

=> To **test** - to run tests in an interactive manner:

```
yarn test
```

The default configuration is to run tests which are related to the files updated since the last commit.

## ``Eject``

When a new app is created using the ``create-react-app``, all of our build settings are actually preconfigured by the tool. Thus, we cannot make any updates to the build setup, e.g. we do not get access to ``webpack.config`` file. It's actually managed by the "``react-scripts``" build dependency.

``eject`` is the way by which we can customize the setup and not get restricted by the configuration provided by Create React App.

```
npm run eject
```

Eject will give use complect control over the config files as well as dependencies like ``Webpack/Babel/ESLint``.

After running the eject command, we can see a '``config``' folder created in our project which has files like ``webpack.config`` for Development and Production plus a ``webpackDevServer.config`` file.

This is how ``package.json`` would look like after running the ``eject`` command (the single build dependency react-scripts is removed from our tutorial project and all the individual dependencies are listed):

```json
{
  "name": "my-react-tutorial-app",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@babel/core": "7.1.0",
    "@svgr/webpack": "2.4.1",
    "babel-core": "7.0.0-bridge.0",
    "babel-eslint": "9.0.0",
    "babel-jest": "23.6.0",
    "babel-loader": "8.0.4",
    "babel-plugin-named-asset-import": "^0.2.2",
    "babel-preset-react-app": "^5.0.2",
    "bfj": "6.1.1",
    "case-sensitive-paths-webpack-plugin": "2.1.2",
    "chalk": "2.4.1",
    "css-loader": "1.0.0",
    "dotenv": "6.0.0",
    "dotenv-expand": "4.2.0",
    "eslint": "5.6.0",
    "eslint-config-react-app": "^3.0.3",
    "eslint-loader": "2.1.1",
    "eslint-plugin-flowtype": "2.50.1",
    "eslint-plugin-import": "2.14.0",
    "eslint-plugin-jsx-a11y": "6.1.1",
    "eslint-plugin-react": "7.11.1",
    "file-loader": "2.0.0",
    "fs-extra": "7.0.0",
    "html-webpack-plugin": "4.0.0-alpha.2",
    "identity-obj-proxy": "3.0.0",
    "jest": "23.6.0",
    "jest-pnp-resolver": "1.0.1",
    "jest-resolve": "23.6.0",
    "mini-css-extract-plugin": "0.4.3",
    "optimize-css-assets-webpack-plugin": "5.0.1",
    "pnp-webpack-plugin": "1.1.0",
    "postcss-flexbugs-fixes": "4.1.0",
    "postcss-loader": "3.0.0",
    "postcss-preset-env": "6.0.6",
    "postcss-safe-parser": "4.0.1",
    "react": "^16.5.2",
    "react-app-polyfill": "^0.1.3",
    "react-dev-utils": "^6.0.3",
    "react-dom": "^16.5.2",
    "resolve": "1.8.1",
    "sass-loader": "7.1.0",
    "style-loader": "0.23.0",
    "terser-webpack-plugin": "1.1.0",
    "url-loader": "1.1.1",
    "webpack": "4.19.1",
    "webpack-dev-server": "3.1.9",
    "webpack-manifest-plugin": "2.0.4",
    "workbox-webpack-plugin": "3.6.2"
  },
  "scripts": {
    "start": "node scripts/start.js",
    "build": "node scripts/build.js",
    "test": "node scripts/test.js"
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "browserslist": [
    ">0.2%",
    "not dead",
    "not ie <= 11",
    "not op_mini all"
  ],
  "jest": {
    "collectCoverageFrom": [
      "src/**/*.{js,jsx}"
    ],
    "resolver": "jest-pnp-resolver",
    "setupFiles": [
      "react-app-polyfill/jsdom"
    ],
    "testMatch": [
      "<rootDir>/src/**/__tests__/**/*.{js,jsx}",
      "<rootDir>/src/**/?(*.)(spec|test).{js,jsx}"
    ],
    "testEnvironment": "jsdom",
    "testURL": "http://localhost",
    "transform": {
      "^.+\\.(js|jsx)$": "<rootDir>/node_modules/babel-jest",
      "^.+\\.css$": "<rootDir>/config/jest/cssTransform.js",
      "^(?!.*\\.(js|jsx|css|json)$)": "<rootDir>/config/jest/fileTransform.js"
    },
    "transformIgnorePatterns": [
      "[/\\\\]node_modules[/\\\\].+\\.(js|jsx)$",
      "^.+\\.module\\.(css|sass|scss)$"
    ],
    "moduleNameMapper": {
      "^react-native$": "react-native-web",
      "^.+\\.module\\.(css|sass|scss)$": "identity-obj-proxy"
    },
    "moduleFileExtensions": [
      "web.js",
      "js",
      "json",
      "web.jsx",
      "jsx",
      "node"
    ]
  },
  "babel": {
    "presets": [
      "react-app"
    ]
  }
}
```

## Unit Testing

Jest is the default testing framework which is automatically configured when we use the Create React App.

Jest can be used for writing unit tests for our individual components to verify that they behave as expected.

Standard file naming convention (Jest will detect them and run our tests):
- .spec.js / .test.js
- all our test files are placed inside the ``tests`` folder

Running ``npm test`` will launch our test runner in watch mode. Thus, every time we make updates to any test file, it would re-run our tests. This is exactly the same behavior as ``npm start``, which recompiles our source code when any of our source files are updated.
