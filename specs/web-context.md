# Web Context

## Requesting context

Context can be requested by dispatching the `contextrequest` event. 

* If at least one context provider is available, a context object with be created on the target element. 
* Each context provider exposes its content on a different key.


```
target.context === undefined; // true
const event = new CustomEvent('contextrequest');
target.dispatchEvent(event);
typeof target.context === 'object'; // true
```
## Providing context

Context can be provided by listening to the `contextrequest` event.

* A `context` object must be added to the target, if it doesn`t already exist.
* A single context value must be added to the target's context using a unique context key.
* Unless you have control on the application context, it is recommended to use a Symbol as the context key.
* The context value can be of any type, but it is recommended to use an object to allow easy extension.
