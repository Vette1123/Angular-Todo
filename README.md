# Angular Todo

A small todo application I built to get a feel for Angular after a long stretch
in React. The problem is simple enough that the framework is the only variable.

## Features

- Add a task from an input, cleared on submit
- Delete a task by id
- Live search that filters the list as you type

## Stack

| | |
| --- | --- |
| Framework | Angular 13 |
| Language | TypeScript |
| Styling | Tailwind CSS + [daisyUI](https://daisyui.com) |
| Tooling | Angular CLI |

## Layout

```
src/app/
├── app.component.*        # shell
├── app.module.ts          # root module, declarations and imports
├── app-routing.module.ts  # router config
└── todo-list/             # the feature: list, input, search, and its state
```

## How it works

State is held directly on `TodoListComponent`: two arrays, the full list and
the filtered view, with the template bound to the filtered one:

```ts
export class TodoListComponent implements OnInit {
  todos: any[]
  filteredTodos: any[]

  add(event: any) {
    if (!event.value) return
    this.todos.push({ id: this.todos.length, name: event.value })
    this.filteredTodos = this.todos
    event.value = ''
  }

  searchTodos(str: string) {
    this.filteredTodos = this.todos.filter((todo) => todo.name.includes(str))
  }
}
```

## Known limitations

This was a first pass at the framework, so these are left as they are:

- **No service, no dependency injection.** State sits on the component and the
  constructor is empty. This is the one Angular idea the project does not
  demonstrate, and the first thing to change if it grows past one screen.
- **`any[]` in a TypeScript project.** A `Todo` interface with `id` and `name`
  costs three lines and would make the template type-checked.
- **`id` is `todos.length`.** Delete something, add something, and two items
  share an id, and then delete removes both.
- **State is not persisted.** A refresh empties the list.
- **No completion state.** Tasks are added, filtered and deleted; there is no
  done flag.

## Notes for anyone coming from React

The repo exists to look at the shape of the framework, not the feature list:

- **A component is four files.** `ng generate component`
  emits a class, a template, a stylesheet and a spec, where React gives you a
  function.
- **Modules are explicit.** A component does not exist until it is declared in
  an `NgModule`. Forgetting that is a common first-day error, and the
  message says what to do about it.
- **Templates are their own language.** `*ngFor`, `*ngIf` and `[(ngModel)]` are
  compiled rather than evaluated as JavaScript, so the Angular compiler understands
  them but ordinary JS tooling does not.
- **Mutation is fine.** `this.todos.push(...)` updates the view. Angular's
  change detection re-reads the binding; there is no new-array requirement the
  way React's referential equality check demands one.

daisyUI sits on top of Tailwind, so I wrote almost no component CSS.

## Running locally

```bash
git clone https://github.com/Vette1123/Angular-Todo.git
cd Angular-Todo
npm install
ng serve
```

Then open `http://localhost:4200`.

```bash
ng build     # production build into dist/
ng test      # unit tests via Karma
```

---

Built by [Mohamed Gado](https://mohamedgado.com) · 2022
