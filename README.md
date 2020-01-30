# 1. Initialise

> npm init

> npm install -D es-dev-server

> npm install lit-html

> npm install todomvc-app-css todomvc-common

`"start" : "es-dev-server --node-resolve"`

Create `index.html` and scaffold `html:5`.

```html
<link rel="stylesheet" href="./node_modules/todomvc-app-css/index.css" />
```

Create `app.js` and link it in the index.html.

```html
<script type="module" src="./app.js"></script>
```

# 2. Template literals

```js
let conference = 'MS Ignite';
let hello = `<h1>Hello ${conference}! It is currently ${new Date().toTimeString()}</h1>`;

document.body.append(hello);
```

```js
let html = (staticParts, dynamicParts) => {
  let el = document.createElement('h1');
  el.innerText = 'WAT?!';
  return el;
};
```

# 3. Lit and render

```js
import { html, render } from 'lit-html';

let conference = 'MS Ignite';

let hello = html`
  <h1>Hello ${conference}!</h1>
  It is currently <b>${new Date().toTimeString()}</b>
`;

render(hello, document.body);
```

# 4. LitElement

```js
import { html, LitElement } from 'lit-element';
```

> Snippet: \_component

```js
class MyApp extends LitElement {
  static get properties() {
    return {};
  }

  constructor() {
    super();
  }

  render() {
    return html`
     
    `;
  }
}

window.customElements.define('my-app', App);
```

```js
this.conference = 'MS Ignite';
```

```html
 <h1>Hello ${this.conference}!</h1> It is currently <b>${new Date().toTimeString()}</b>
```

```html
<my-app></my-app>
```

# 5. TodoMVC App

```js
return {
  todos: Array
};
```

```js
  constructor() {
    super();

    this.todos = [ ];
  }
```

> Snippet: \_html_app

```html
<section class="todoapp">
  <header class="header">
    <h1>todos</h1>
  </header>

  <section class="main"></section>
</section>
```

```js
  createRenderRoot() {
    return this;
  }
```

# 6. Show the list

> Snippet: \_data

```js
  { title: 'Initialise the app', completed: true },
  { title: 'Create a todo', completed: true },
  { title: 'List the todos', completed: true },
  { title: 'Mark a todo as completed', completed: false },
  { title: '????', completed: false },
  { title: 'Profit!', completed: false },
```

```html
<ul class="todo-list">
  ${this.todos.map((todo) => html`
    <view-todo .todo="${todo}"></view-todo>
  `)}
</ul>
```

```js
import './view-todo';
```

Create `view-todo.js`.

> Snippet: \_component

```js
import { html, LitElement } from 'lit-element';

class ViewTodo extends LitElement {
  createRenderRoot() {
    return this;
  }

  static get properties() {
    return {};
  }

  render() {
    return html``;
  }
}

window.customElements.define('view-todo', ViewTodo);
```

```js
static get properties() {
  return { todo: Object };
}
```

> Snippet: \_html_view_todo

```html
<li>
  <div class="view">
    <input class="toggle" type="checkbox" />
    <label></label>
  </div>
</li>
```

```js
<label>${this.todo.title}</label>
```

# 7. Add Todo

> Create file `add-todo.js`.

> Snippet: \_component

```js
import { html, LitElement } from 'lit-element';

class AddTodo extends LitElement {
  createRenderRoot() {
    return this;
  }

  static get properties() {
    return {};
  }

  render() {
    return html``;
  }
}

window.customElements.define('add-todo', AddTodo);
```

> Snippet: \_html_add_todo

```html
<input class="new-todo" placeholder="What needs to be done?" autofocus="" />
```

```html
@keyup="${(e) => this.handleAddTodo(e)}"
```

> Snippet: \_handleAddTodo

```js
  handleAddTodo(e) {
    if (e.key !== 'Enter') return;

    this.addTodo({
      title: e.target.value,
      completed: false
    });

    e.target.value = '';
  }
```

```js
  static get properties() {
    return {
      addTodo: Object
    };
  }
```

Add the corresponding side on `app.js`.

```js
  addTodo(todo) {
    this.todo = [...this.todos, todo];
  }
```

```html
<add-todo .addTodo="${(todo) => this.addTodo(todo)}"></add-todo>
```

```js
import './add-todo.js';
```

# 8. Mark a todo as done

```js
    ?checked="${this.todo.completed}"
    @click=${(e) => this.toggleCompleted(this.todo)}
```

```js
  static get properties() {
    return {
      todo: Object,
      toggleCompleted: Object
    };
  }
```

```html
<view-todo .todo="${todo}" .toggleCompleted="${(todo) => this.toggleCompleted(todo)}"></view-todo>
```

```js
  toggleCompleted(todo) {
    //
  }
```

```js
  toggleCompleted(todo) {
    this.todos = this.todos.map((t) => {
      if (t !== todo) return t;

      return {
        ...todo,
        completed: !todo.completed
      };
    });
  }
```
