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

## What's Included
- Everything you need for typical web app
- Templates, Routing, HTTP, Dependency Injection, TypeScript
- Official style guide (John Papa).
- Opinionated. Easy to jump into a new project
- Can still mix and match (rendering, routing, etc)

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
- Familiar to most devs and designers

## Change detection and DOM updates
- How does it work? Angular maintains a directed tree of components and their relationships. Stable, fast, predictable.
- By default, Angular check all components in the tree with each event.
- With immutables, Angular can check only subtrees where an input has changed (onPush). Even faster

## Data store
- Optional: ngrx/store
- Entire application state in one immutable data structure
- Reducer functions take you from one state to the next
- Use with onPush change detection for increased speed
- Can also use Redux (or any other store you prefer)

## Types with TypeScript
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
```

- Improve function legibility and error checking:
```
function addAmountToEachItem(amount: number, items: number[]): number[] {
  return items.map(i => i + amount);
}
```

- VS Code or atom-typescript provide great linting, jump to definition, autofill, peek features

## Observables
- Moving beyond promises to handle streams of data
- Provides a rich set of FP tools like Underscore, but for streams of data
- Angular's router and HTTP services use them
- Many operators available: map, filter, merge, scan, etc
- Subscribe to websocket or mouse movements + transform stream
- Simple example:
```
const testSubject = new Rx.Subject();
const basicScan = testSubject
  .startWith(0)
  .scan((acc, curr) => acc + curr);
const subscribe = basicScan.subscribe(val => console.log('Accumulated total:', val));
testSubject.next(1); //1
testSubject.next(2); //3
testSubject.next(3); //6
```
(Source: https://gist.github.com/btroncone/d6cf141d6f2c00dc6b35)

## Testing
- Dependency Injection (easy mocking, clean separation between modules).
- Solves the banana/gorilla/jungle problem with testing
- In this example, the PersonService is injected:

```
export class personListComponent {
  people: Person[];
  constructor(personService: PersonService) {
    this.people = peopleService.getHeroes();
  }
}

- Inject the real PersonService, or a mocked version. Component doesn't know the difference
```

- When testing the component, mock the PersonService and inject the mock
- Frequently used with http services
-  You can use Angular's DI in React too (see http://blog.mgechev.com/ for a post on it)

## Mobile
- Two options for mobile apps: Ionic and NativeScript
- Both have cli tools -- easy to get started
- Ionic: mobile apps with Angular and Cordova (Ionic 2 uses Angular 2)
- Ionic interface components = HTML/CSS/JS
- NativeScript: Angular 2 native apps with native interfaces
- NativeScript is fast, but there's more to maintain

## Trends
- Small, tightly-scoped components with no side effects
- Component tree with DOM diffing
- A single source of state
- Strong types
- Functional or reactive programming
