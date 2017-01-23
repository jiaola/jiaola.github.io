---
layout: "post"
title: "Set up environment variables with webpack for React"
date: "2017-01-23 11:39"
tags: [webpack]
comments: true
---

To pass on an environment variable to the react application, use the define plugin.

```
plugins.push(new webpack.DefinePlugin({
  'process.env': {
    NODE_ENV: JSON.stringify(process.env.NODE_ENV || 'developemnt')
  }
}));
```

This will pass the NODE_ENV variable, and in the code, it can be referred as `process.env.NODE_ENV`:

```
if (process.env.NODE_ENV === 'production') {
  // code for production
} else if (process.env.NODE_ENV === 'development') {
  // code for development
}
```

<!-- more -->

The environment variable can be set in the package.json.

```
...
"scripts": {
  "build:development": "NODE_ENV=development webpack -p",
  "build:production": "NODE_ENV=production webpack -p"
}
...
```

Now simply run the following when build for production.
```
npm run build:production
```

Note if you see the following messages in the browser console, it means that the environment variables are not set up correctly.

```
Warning: It looks like you're using a minified copy of the development build of React. When deploying React apps to production, make sure to use the production build which skips development warnings and is faster. See https://fb.me/react-minification for more details.

You are currently using minified code outside of NODE_ENV === 'production'. This means that you are running a slower development build of Redux. You can use loose-envify (https://github.com/zertosh/loose-envify) for browserify or DefinePlugin for webpack (http://stackoverflow.com/questions/30030031) to ensure you have the correct code for your production build.
```
