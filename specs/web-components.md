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


```js
class ProductItem extends HTMLElement {
    
}
```


```html
<h1>{{pageTitle}}</h1>
<div><input bind-value="filter"></div>
<ul>
  <book-item items="items"></book-item
</ul>
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


```
