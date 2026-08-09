# Angular Todo

A small todo application built to get an honest feel for Angular — written
after a long stretch in React, on a problem simple enough that the only
variable is the framework.

## Features

- Add a task, mark it complete, delete it
- Completed tasks render struck through rather than disappearing
- Component-scoped styles, no global CSS beyond the Tailwind and daisyUI setup

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
└── todo-list/             # the feature: list, item, and its state
```

## Notes for anyone coming from React

The feature list is the boring part. The reason this repo exists is the shape
of the framework around it:

- **A component is four files, not one function.** `ng generate component`
  emits a class, a template, a stylesheet and a spec. React would have handed
  you a function and left the rest to you. Neither is wrong; they optimise for
  different team sizes.
- **State lives in a service, injected through the constructor.** There is no
  hook and no provider tree to thread — you declare what you need as a
  constructor parameter and the framework resolves it. Dependency injection is
  the framework's, not a library you chose.
- **Modules are explicit.** A component does not exist until it is declared in
  an `NgModule`. Forgetting that is the single most common first-day error, and
  the error message says exactly what to do about it.
- **Templates are their own language.** `*ngFor`, `*ngIf` and `[(ngModel)]` are
  compiled, not JavaScript expressions, so the type checker sees through them
  but your editor's JS tooling does not.

daisyUI sits on top of Tailwind so none of the effort went into writing
component CSS.

## Running locally

```bash
git clone https://github.com/Vette1123/Angular-Todo.git
cd Angular-Todo
npm install
ng serve
```

Then open `http://localhost:4200`. The app reloads on save.

```bash
ng build     # production build into dist/
ng test      # unit tests via Karma
```

---

Built by [Mohamed Gado](https://mohamedgado.com) · 2022
