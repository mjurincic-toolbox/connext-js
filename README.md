![Connext logo](https://i.ibb.co/kJpDpQG/connextlogo.png)

A lightweight middleware and route handling solution for Next.js. Powered by [dirext](https://github.com/dirext-js/dirext) 🛸

# Install connext
`$ npm install connext-js`

Initialize a new instance of Connext. 
```javascript
const Connext = require('connext-js');
const app = Connext();
```

# Usage

Connext is a middleware solution for Next.js with Express-style syntax that supports both global and flexible route-specific middleware. For global middleware, you must create a `controllers` folder that must contain a `global.js` controller file. We recommend also creating controller files for your other middleware as a way to modularize your API logic. 

## Setting Routes
Setting routes using `connext`closely resembles setting routes in Express. 

### Method
All valid HTTP request methods have associated methods on the connext object. 
```javascript
app.get();
app.post();
app.put();
app.delete();
app.head();
app.trace();
app.patch();
app.options();
app.connect();
```

### URL
Connext supports any static or queried API endpoint. Support for dynamic routing is coming soon. ⚡️

### Request & Response objects
In `connext`, you have access to the **request object** available in Next.js and that access persists through the middleware chain- just like Express! The available properties are `req.url`, `req.method`, and `req.body`.

Unlike Express, if you need to store data you can add any key to the **response object** with whatever data you wish to store. This will simply augment your response object and continue to persist this object throughout the lifecycle of the request. 
EX:
```javascript
response.data = JSON.stringify(data);
response.match = true;
response.array = ['I'm the data you need'];
```

## Middleware
Example file structure:
```
├── ...
├── controllers
│   ├── global.js         # required for global middleware
│   └── middleware.js     # suggested for modularization of middlware functions                                 
├── pages                 # required folder for routes in Next.js 
│   └── api               # required folder for API routes in Next.js 
└── ...                   
```

### Global Middleware
To utilize Connext's global middleware functionality, you must create a `global.js` file in a folder called `controllers`. The `controllers` folder must be at the same level as your `pages` folder and export an **array**. 

`connext` has a simple global built-in error handler that will run whenever something is passed into the invocation of `next()`. If you'd like to use your own error handler, define it in `global.js` as the **last** element of the exported array. 

**global.js example**
```javascript
// a global middleware function
const globalOne = (req, res, next) => {
  console.log('this the first function that runs!');
  return next();
};
// another global middleware function
const globalTwo = (req, res, next) => {
  console.log('another one!');
  return next();
};
// global error handler
const errorHandler = (err, req, res, next) => {
  return res.status(500).send('an error occurred');
};
// export your array of global middleware
module.exports = [globalOne, globalTwo, errorHandler];
```

We recommend that you modularize your other middleware in one or more files in your `controllers` file to keep your code readable and easy to debug 🐞

**middleware.js example**
```javascript
const middlewareController = {};

middlewareController.functionOne = (req, res, next) => {
  // middleware functionality here
  return next();
}

middlewareController.functionTwo = (req, res, next) => {
  // middleware functionality here
  return next();
}

module.exports = middlewareController;
```

### connext.METHOD(url, [...middleware])

Like in express, every method in Connext has a string bound to it that coresponds with a valid HTTP method. 

For example: `GET, DELETE, POST`, etc.










