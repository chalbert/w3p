# Web Actions

## Dispatching actions

Actions can be dispatched as an `action` event.

* The custom event detail **must** be defined as an object.
* A unique `type` field **must** be added to the detail object.
* The custom event **can** bubble, in which case it is a **public** action.
* If the custom event *does't* bubble, it is **private** action.
* Data **may** be added as the `data` field of the detail object.
* It is **recommended** that the `data` field is set as an object to allow easy extension.

```js
const event = new CustomEvent('action', {
  bubbles: true,
  detail: {
    type: 'selectItem',
    data: {
      item
    }
  }
});
actor.dispatchEvent(event);
```

## Actor interface

A component **may** become an `actor` by implementing the `Actor` interface.

```js
dispatchAction(type: string, data) : boolean;
```

It can then be used thus:

```js
this.addEventListener('click', () => this.dispatchAction('selectItem', this.item));
```

## Action behaviors

Behaviors matching `{event}-action="type"`. 

* An event listenner for the `event` **must** be added. 
* When the event is captured, an action of the specified type **must** be dispatched.
* The data of the event **must** include the original event object (`data: { event }`).
* Action behaviors **only** dispatch **private** action.

```html
<button click-action="selectItem">Select</button>
```

