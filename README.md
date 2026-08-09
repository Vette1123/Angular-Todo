# Angular Todo

A small todo application built to get an honest feel for Angular — written
after a long stretch in React, on a problem simple enough that the only
variable is the framework.

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

State is held directly on `TodoListComponent` — two arrays, the full list and
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

Kept visible rather than tidied away — this was a first pass at the framework:

- **No service, no dependency injection.** State sits on the component and the
  constructor is empty. That is the one Angular idea this project does *not*
  demonstrate, and the first thing to change if it grows past one screen.
- **`any[]` in a TypeScript project.** A `Todo` interface with `id` and `name`
  costs three lines and would make the template type-checked.
- **`id` is `todos.length`.** Delete something, add something, and two items
  share an id — after which delete removes both.
- **State is not persisted.** A refresh empties the list.
- **No completion state.** Tasks are added, filtered and deleted; there is no
  done flag.

## Notes for anyone coming from React

The feature list is the boring part. The reason this repo exists is the shape
of the framework around it:

- **A component is four files, not one function.** `ng generate component`
  emits a class, a template, a stylesheet and a spec. React would have handed
  you a function and left the rest to you.
- **Modules are explicit.** A component does not exist until it is declared in
  an `NgModule`. Forgetting that is the most common first-day error, and the
  message says exactly what to do about it.
- **Templates are their own language.** `*ngFor`, `*ngIf` and `[(ngModel)]` are
  compiled rather than evaluated as JavaScript, so the Angular compiler sees
  through them but ordinary JS tooling does not.
- **Mutation is fine.** `this.todos.push(...)` updates the view. Angular's
  change detection re-reads the binding; there is no new-array requirement the
  way React's referential equality check demands one.

daisyUI sits on top of Tailwind so none of the effort went into component CSS.

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
