---
layout: post
title: Initilize a ReactJS project with Typescript from scratch
description: Initilize a ReactJS project with Typescript from scratch
keywords: reactjs, webpack, typescript
---

Create a new directory for the project and then go inside it.

```bash
mkdir sunflora
cd sunflora
```

Initialize git to keep tracking changes we're gonna make.

```bash
git intit
```

Add `package.json` file for package management.

```bash
touch package.json
```

And then, add the following content to just created package.json file.

```bash
{
  "license": "MIT"
}
```

Add two most important packages for any ReactJS project.

```bash
yarn add react react-dom
yarn add -D @types/react @types/react-dom
```

As we're gonna use Typescript, we need to add those typing packages as devDependencies.

One more thing that, we don't need upload a huge `node_modules` to the git repository. 

![meme](https://user-images.githubusercontent.com/25602820/123408396-2af02c00-d5d7-11eb-9bca-df65ff12d8a6.png)

We ignore it by adding to `.gitignore` file.

```bash
echo "node_modules" >> .gitignore
```

Create a new src directory, this directory will be the main directory of the project.

```bash
mkdir src
touch App.tsx
```

Add some content to `App.tsx`. We're expecting to print out the text `Hello from ReactJS` when user access our ReactJS app.

```tsx
// src/App.tsx

import React from 'react';

const App: React.FC = () => {
  return (
    <h3>Hello from ReactJS</h3>
  )
}

export default App;
```

Next we need an entrypoint where we're gonna mount our app to the DOM, our app will be mounted into a `<div id="root"></div>`. Move to next section to see how to add this element to HTML page.

```bash
touch index.tsx
```

```tsx
// src/index.tsx

import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

### Configure webpack and Typescript

We need to add wepack as source bundler, we need to add weback config file as following.

```tsx
// webpack.config.ts

const path = require("path");

module.exports = {
  output: {
    path: path.resolve(__dirname, "build"),
    filename: "bundle.js",
  },
  entry: {
    index: path.join(__dirname, "src/index.tsx"),
  },
  resolve: {
    extensions: [".ts", ".tsx", ".js"],
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: "ts-loader",
        exclude: /node_modules/,
      },
    ],
  }
}
```

- `output` - indicates where to put the result after bundling.
- `entry` - the entrypoint which webpack will come to here firstly to bundle code.
- `resolve` - as we support Typescript, we make sure webpack understand what we expect to do when it see `import App from './App'`.
- `rules` - specfies which loader will handle file extension, we will use the `ts-loader` to bundle `ts` and `tsx` file. Just add it to devDependencies:

```bash
yarn add -D ts-loader
```

The `ts-loader` needs `tsconfig.json` file to config the Typescript compiler.

```tsx
// tsconfig.json
{
  "compilerOptions": {
    "noImplicitAny": true,
    "module": "es6",
    "target": "es5",
    "jsx": "react",
    "allowJs": true,
    "moduleResolution": "node",
    "allowSyntheticDefaultImports": true
  }
}
```

### webpack-dev-server and html-webpack-plugin

```bash
yarn add -D webpack-dev-server html-webpack-plugin
```

We need a `wepack-dev-server` to serve the request to our app, and inside the HTML file we need an ⚓ where our app will mount to.

```html
<!-- public/index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta
      name="description"
      content="ReactJS from scratch"
    />
    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

As you can see, there is `script` tag here to inject the bundled code here. And this is where `html-webpack-plugin` saves us.

```diff
+ const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
...
+  plugins: [
+    new HtmlWebpackPlugin({
+      hot: true,
+      template: "./public/index.html",
+    }),
+  ],
+  devServer: {
+    contentBase: path.join(__dirname, "build"),
+    compress: true,
+    port: 8080,
+  },
};
```

### Final step

Just add some scripts to `package.json` file to build and start webpack-dev-server

```json
// package.json
{
	"scripts": {
		"build": "webpack --config ./webpack.config.ts",
    "start": "webpack serve --env development --open"
	}
}
```

Now, we just need to run `yarn start` and access `[http://localhost:8080](http://localhost:8080)`  to see the result.

All code is available on [Github repository](https://github.com/namtx/sunflora).

