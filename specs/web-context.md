# Web Context

## Requesting context

Context can be requested by dispatching the `contextrequest` event. 

* The custom event **must** bubble
* If at least one context provider is available, a context object with be created on the consumer element. 
* Each context provider exposes its content on a different key.

```js
consumer.context === undefined; // true
const event = new CustomEvent('contextrequest', {
  bubbles: true
});
consumer.dispatchEvent(event);
typeof consumer.context === 'object'; // true
```
## Providing context

Context can be provided by listening to the `contextrequest` event.

* A `context` object **must** be added to the target, if it doesn't already exist.
* A single context value **must** be added to the target's context using a unique context key.
* Unless you have control on the application context, it is recommended to use a Symbol as the context key.
* The context value can be of any type, but it is recommended to use an object to allow easy extension.

```js
const contextKey = Symbol('intl');
provider.addEventListener('contextrequest', (event) => {
  event.target.context = event.target.context || {};
  event.target.context[contextKey] = {
    locale: 'fr'
  };
});
```

## Immutable context

Although it is not required, it is recommended to make your context objects immutable so that consumer cannot alter their context directly. You can either use a library, or use `Object.freeze`.

* Intermediate provider should only freeze the context under their own key
* Only the root context should freeze the context object itself
* It is recommended to only use serializable structure
* If you share your provider outside your app :
  * It may be wiser to let the application decide if the context should be frozen
  * You should stay away from library to format your context

```js
const contextKey = Symbol('intl');
intermediateProvider.addEventListener('contextrequest', (event) => {
  event.target.context = { ...event.target.context };
  event.target.context[contextKey] = Object.freeze({
    locale: 'fr'
  });
});

rootProvider.addEventListener('contextrequest', (event) => {
  Object.feeze(event.target.context);
});

```

## devconfig

```json
"context": {
  "guards": [
    "symbol",
    "object",
    "serializable"
  ],
  "transforms": [
    "freeze",
    "deepfreeze",
    "immutable
  ]
}
```


