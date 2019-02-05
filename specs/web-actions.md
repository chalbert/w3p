# Web Actions

## Dispatching actions

Actions can be dispatched as an `action` event.

* The custom event must bubble
* The custom event detail must be defined as an object
* The type of action must be set as the `type` field of the detail object
* Data can be aded as the `data` field of the detail object
* It is recommended that the `data` field is set as an object to allow easy extension

```js
const event = new CustomEvent('action', {
  bubbles: true,
  detail: {
    type: 'ITEM_SELECTED',
    data: {
      item
    }
  }
});
actor.dispatchEvent(event);
```

