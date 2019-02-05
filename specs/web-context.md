# Web Context

## Requesting context

Context can be requested by dispatching the `contextrequest` event. 

* The custom event **must** bubble
* The full context will added to `detail` of the `contextrequest` event itself.

```js
const event = new CustomEvent('contextrequest', {
  bubbles: true
});
consumer.dispatchEvent(event);
const { context } = event.detail;
```

## Context change

When a context changes, the `contextchange` event must be dispatched on the `window` object.

* The `type` property of `detail` must correspond to the context key of the context that has changed
* The `context` property of `detail` must to the context that has changed
* One event is triggered for each context that changes

Listenning to context changes:

```js
window.addEventListenner('contextchange', (event) => {
  const { type, context } = event.detail;
});
```

## Providing context

Context can be provided by listening to the `contextrequest` event.

* If a `types` property is defined on the detail of the `contextrequest` event, the provider should only anwser if its type its key is included in the list.
* A `context` object **must** be added to the target, if it doesn't already exist.
* A single context value **must** be added to the target's context using a unique context key.
* Unless you have control on the application context, it is recommended to use a Symbol as the context key.
* The context value can be of any type, but it is recommended to use an object to allow easy extension.
* A component providing context **should** include a `contextType` property corresponding to the context key.
* A single component **should not** provide more than one context.

```js
const contextKey = Symbol('intl');
provider.addEventListener('contextrequest', (event) => {
  event.target.context = event.target.context || {};
  event.target.context[contextKey] = {
    locale: 'fr'
  };
});
```

When the context changes

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


