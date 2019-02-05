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

