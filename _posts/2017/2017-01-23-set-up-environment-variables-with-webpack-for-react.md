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
