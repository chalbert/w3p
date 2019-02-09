# Web components



## Rendering and binding

Use cases:

* Repeats
* Rendering
* Formatting
* Binding
* Two-way binding (or equivalent)
* Conditions
* Show hide

* Allow for multiple approach to rendering.
* Use `devconfig` to restrict which approaches are acceptable inside a project.

### Behavior based (+ mustache)

```html
<h1>{{pageTitle}}</h1>
<div><input bind-value="filter"></div>
<ul>
  <li render-for="book in books | filterBy: filter">
      {{book.title | allCap}} by {{book.author}}
  </li>
</ul>
```

### Template string 

```html
<h1>${pageTitle}</h1>
<div><input value="${filter}" input-action="updateFilter"></div>
<ul>
  ${books.filter(bookFilter).map((book) => {
    return `<li>${formatTitle(book.title)} ${book.author}</li> 
  })}
</ul>
```

### JSX

```jsx
<h1>{pageTitle}</h1>
<div><input value="{filter}" onClick={updateFilter}></div>
<ul>
  {books.filter(bookFilter).map((book) => {
    return <li>{formatTitle(book.title)} {book.author}</li> 
  })}
</ul>
```

### HOC

* Composition is nicer, but we don't to create a pyramid of doom.
  ```html
  <with-intl>
    <with-connect>
      <with-reducer>
        <with-saga>
          <my-component></my-component>
        </with-saga>
       </with-reducer>
    </with-connect>
  </with-intl>
  ```
  An alternative would be to use shadow-dom to offuscate the wrappers. The good points about visible wrapper is that it is very transparent and easy to debug. The downside could be its performance, but it would be worth do a real perf analysis. Usability is more important than pure perf.
* Inheritance *could* work with strong guidelines, although is *feels* wrong when use to real composition.
  * Only public `constructor` method authorized

```js
function withIntl(WrappedItem) {
  return class WithIntl extends WrappedItem {
    constructor(...args) {
      super(...args);
    }
  }
}
```

Manual extensions

```js
function withIntl(element) {
  const event = new CustomEvent('contextrequest', {
    detail: {
      types: ['intl']
    }
  });
  element.dispatch(event);
  const { intl } = event.detail.context;
  element.intl = intl;
  element.addEventListenner('contextchange', (event) => {
    if (event.detail.type === 'intl') {
       const { intl } = event.detail.context;
      element.intl = event.;
    }
  });
}

class ProductItem extends HTMLElement {
  constructor(args) {
    super(..args);
    withIntl(this);
    // call each hocs
  }
}
```

* Behaviors

```html
class ProductItem extends HTMLLIElement {
  template = `${this.name} by ${this.author}`
}

// not super readable...
class ProductList extends HTMLElement {
  template = () => `
    <ul>${itemsTemplates()}</ul>
  `;

  itemsTemplates = () => this.items.map(item => this.itemTemplate(item))

  itemTemplate = (item) => `
    `<li>${item.name} by ${item.author}</li>`
  `;
} 
```

**There's probably no good way to do heavy composition without scoped component registry...**

class BookList extends HTMLULElement {
  template = '<book-item></book-item>';
}


```html
<h1 render-text="pageTitle"></h1>
<div><input bind-value="filter"></div>
<product-list></product-list>
```



```js

class ProductList extends HTMLElement {
  template = template`
    Allo ${this.name}
    <ul>
      <template>
         <li >
      </template>
    </ul>
  `;
}


How to set html attribute?

```
<input render-attr-disabled="isActive">
```

## Rendering low level component

Could the solution be to **not** use a templating engine? Let's remember one of the Platform vow is to reduce the abstraction level to the minimum, ideally with no abstraction at all.

Could we train developer into using the most efficient manual dom manipulation, maybe with some help provided by the tooling?

Yes, it's longer to write a low level component this way. But having a component completly decoupled form any dependency is a greate asset.

Event better, use a tool that help generate, standard compliant, 0 abstraction code. The final code is not hidden, but introduce to the developer. Component can be created either by code or by the tools, and the tool is clever enough to understand the code.  **Tools are the abstraction**.

```js
class LowLevelComponent extends HTMLElement {  
  connectedCallback() {
    this.empty();
    this.render();
    this.addEventListenners();
  }
  
  disconnectedCallback() {
    this.removeEventListenners();
  }
  
  render() {    
    this.appendChild(this.renderHeader());
    this.appendChild(this.renderActionBar());
  }
  
  empty() {
    while (this.firstChild) {
      this.removeChild(this.firstChild);
    }
  }
  
  renderHeader() {
    const header = document.createElement('header');
    const title = document.createElement('h1');

    const titleText = document.createTextNode(this.state.title);
    h1.appendChild(titleText);
    
    return header;
  }
  
  renderActionBar() {
    const actionBar = document.createElement('div');
    const actionButton = document.createElement('button');
    actionBar.appendChild(actionButton);  
    
    this.refs = { ...this.refs, actionButton }
    
    return actionBar;
  }
  
  addEventListeners() {
    this.refs.button.addEventListener(this.onClick);
  }
  
  removeEventListenners() {
    this.refs.button.removeEventListener(this.onClick);
  }
  
  onClick = (event) => {
  
  }
}
```

The idea woud be to select the level of abstraction of the templating according to the level of abstraction of the component. Low level component would use low level dom manipultion, high level business component would use high level templating. And in between you would see a transition.

## Passing data to subcomponents

* Attribute : string only
* Property : most direct way to pass and get data. Can use getter/setter.
* Method : A bit more complex, allow hidden structure. Do we want to hide structure? What benefit over getter/setter. More consistant to always use property?
* Sub components.
* Script or template containing js/json.
* Event providing data
* Event requesting data

Primitive data : attribute, always mapped to property. Allow type casting.
Rich data : property.

Direct data (property) and indirect data (context).

When a component is using a  context, it is said to be *connected* to a context.

