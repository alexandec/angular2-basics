# Angular 2 Basics
An introduction to Angular 2

## Getting started
- Angular 2 is a total reboot
- Angular CLI: create an app with best practices
- Simplest way to get started:

```
npm install -g @angular/cli
ng new PROJECT_NAME
cd PROJECT_NAME
ng serve
```

- Your app is running at localhost:4200
- ng generate: scaffolding for components, pipes, services, etc
- what else: `ng build` (webpack), `ng test` (jasmine/karma), `ng e2e` (protractor), `ng lint`, `ng build`, `ng eject`

## What's included
- Routing, dependency injection, TypeScript, server-side rendering
- Official style guide (John Papa).
- Opinionated. Easy to jump into a project
- But, you can mix and match if you like

## Components
- Instead of MVC, encapsulate everything inside small, simple components
- Here's how it looks:

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: '<h1>{{title}}</h1>',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title: string = 'This is a string.';
}
```

- Components should be small, simple, encapsulated, easy to test
- Each component gets its own scope (JS and CSS)
- Use services to store/retrieve data

## Communication between components
**1. Parent -> child input binding.** Parent template contains:

```
<child-component [myInput]=3></child-component>
```

Child component:

```
export class child-component { @Input() myInput: number; }
```

**2. Parent listens for event from child.** Parent component template:

```
<child-component (myInput)="processInput($event)"></child-component>
```

Child component:

```
export class childComponent {
  @Output() myOutput: new EventEmitter<number>();
  ...
  this.myOutput.emit(3);
}
```

## Templates
- Example of a basic template:

```
<ul>
  <li *ngFor="let item of items" (click)="itemClick(item)">Name: {{item.name}}</li>
</ul>
```
- It's HTML with enhancements: `*` changes UI, `()` inputs, `[]` outputs, `([])` for two-way bindings, and `{{}}` for data bindings.

## Change detection and DOM updates
- How does it work? Angular maintains a directed tree of components and their relationships. Stable, fast, predictable.
- By default, Angular checks all components in the tree with each event.
- With immutables, Angular can check only subtrees where an input has changed (onPush). Even faster

## Data store
- Optional: ngrx/store
- Entire application state in one immutable data structure
- Reducer functions take you from one state to the next
- Use with onPush change detection for increased speed
- Can also use Redux (or any other store you prefer)

## Types
- TypeScript: compiler which adds strong typing to JS (you get ES6 too). `.ts -> .js`
- You can start with an existing codebase and gradually add types. It looks like:

```
let myNumber: number = 42;
let author: string = 'Douglas Adams';
author = 5; // Compile time error: Type 'number' is not assignable to type 'string'
```

- Can also create object interfaces like:

```
interface Bird {
  name: string;
  speed: number;
}

let myBird: Bird = {
  name: 'pigeon',
  speed: 'fast' // Error
}
```

- VS Code or atom-typescript provide great linting, jump to definition, autofill features

## Testing
- Dependency Injection (easy mocking, clean separation between modules).
- In this example, the PersonService is injected:

```
@Component({
  selector: 'person-list',
  providers: [PersonService]
})
export class personListComponent {
  people: Person[];
  constructor(personService: PersonService) {
    this.people = peopleService.getPeople();
  }
}
```

- When testing the component, mock the PersonService and inject the mock
- The component doesn't know the difference
- Frequently used with http services
- You can use Angular's DI in React too (see http://blog.mgechev.com/ for a post on it)

## Trends
- Small, tightly-scoped components with no side effects
- Component tree with DOM diffing
- A single source of state
- Strong types
- Functional or reactive programming
