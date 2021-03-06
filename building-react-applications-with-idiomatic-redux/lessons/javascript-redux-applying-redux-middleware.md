In the previous lesson, we figured out the contract that we want to use for our middleware functions. However, middleware wouldn't be very useful if everybody had to implement `wrapDispatchWithMiddlewares` on their own. This is why I'm removing it, and instead I'm going to import a utility called `applyMiddleware` from **Redux**. 

**configureStore.js**
```javascript
import { createStore, applyMiddleware } from 'redux';
```
I'm going back to my `configureStore` function. I notice that I don't need the `store` right away. I can move the `store` creation after specifying the middleware. I can also remove my custom function, and instead I will `createStore` with the middleware right away. I am passing the second argument to `createStore`, which is the result of calling apply middleware with my middleware functions as positional arguments. 

**configureStore.js**
```javascript
const configureStore = () => {
  const middlewares = [promise];
  if (process.env.NODE_ENV !== 'production') {
    middlewares.push(logger);
  }

  const store = createStore(
    todoApp,
    applyMiddleware(...middlewares)
  );
  return store.
}
```
This last argument to create store is called an enhancer, and it's optional. If you want to specify the `persistentState`, you need to do this before the enhancer. You can also keep the `persistentState` if you don't have.

Many `middlewares` are available as **npm packages**. Both the `promise` middleware and the `logger` middleware are no exceptions to this. I am opening up a terminal, and I'm running `npm install --save redux-promise`, which is a middleware that implements the promise support.

**terminal**
```bash
$ npm install --save redux-promise
```
I can install a package called `redux-logger` in the same way, which is similar to the `logger` middleware we wrote before, but it's more configurable. 

**terminal**
```bash
$ npm install --save redux-logger
```
I am adding an input called lowercase `promise` that corresponds to the `redux-promise` middleware, and I'm inputting `createLogger` from `redux-logger`. 

**configureStore.js**
```javascript
import promise from 'redux-promise';
import createLogger from 'redux-logger';
```
It lets you configure the middleware before creating it. I won't pass any options to gather default configuration. Finally, since I don't need to reference the store anymore, I'll just return it directly from `configureStore`.

**configureStore.js**
```javascript
const configureStore = () => {
  const middlewares = [promise];
  if (process.env.NODE_ENV !== 'production') {
    middlewares.push(createLogger());
  }

  return createStore(
    todoApp,
    applyMiddleware(...middlewares)
  );
};
```
Let's recap how to apply Redux middleware. We use the function called `applyMiddleware` that we import from Redux. It accepts the middleware functions as positional arguments. For example, we could have written `promise`, `createLogger`. However, we only want to apply the logger conditionally. If we're not running in the production environment, we will push the logger to the array of middlewares. We use the [ES6 spread operator](https://egghead.io/lessons/ecmascript-6-using-the-es6-spread-operator) to pass every middleware as a positional argument to apply middleware.

The `applyMiddleware` returns something called an **enhancer**. This is the optional last argument to create store. If you want to also supply the `persistentState`, you need to do this before the enhancer.