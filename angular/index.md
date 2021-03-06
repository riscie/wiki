![](./0.png)
# angular

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=3 orderedList=false} -->

<!-- code_chunk_output -->

* [angular](#angular)
	* [functions vs. pipe](#functions-vs-pipe)
		* [why?](#why)
		* [careful with object-inputs in pipes!](#careful-with-object-inputs-in-pipes)
		* [source](#source)
	* [routing](#routing)
		* [tracing / debugging](#tracing-debugging)
	* [using jest with angular and angular-cli](#using-jest-with-angular-and-angular-cli)
		* [installation](#installation)
		* [config](#config)
		* [removing all auto-generated spec files](#removing-all-auto-generated-spec-files)

<!-- /code_chunk_output -->


## functions vs. pipe
This will get called **many times more often**
```html
<!-- using a function -->
<tr *ngFor="let player of players">
    <td>{{getScore(player)}}</td>
</tr>
```

than this
```html
<!-- using a pipe -->
<tr *ngFor="let player of players">
    <td>{{player | rating}}</td>
</tr>
```

### why?
* angular will remember the input value of the pipe and **cache** the output
* if the input value has not changed, it will use the cached value

### careful with object-inputs in pipes!
* if a pipe takes an object as an input and inside this object a primitive changes, angular will reevaluate the output
* it's a good idea to **keep pipe inputs as primitive as possible**
* or use **immutable objects**

### source
* [ngconf 2018 - Increasing Performance - more than a pipe dream - Tanner Edwards](https://www.youtube.com/watch?v=I6ZvpdRM1eQ)

## routing
### tracing / debugging
#### enable tracing
* we can enable tracing for debug purposes with the help of `{enableTracing: true}`

**app.routing.ts**
```ts 
import { RouterModule, Routes } from '@angular/router';
import { ModuleWithProviders } from '@angular/core';

const routes: Routes = [//...];

export const ModuleRouting: ModuleWithProviders = RouterModule.forRoot(routes, {enableTracing: true});
```

#### filtering down tracing
* normal tracing can be too verbose. we can filter it down a bit more like so: (disable tracing first)
* Possible Event Classes
  * `NavigationStart`
  * `NavigationEnd`
  * `NavigationCancel`
  * `NavigationError`
  * `RoutesRecognized`
  * `RouteConfigLoadStart`
  * `RouteConfigLoadEnd`
  
**app.component.ts**
```ts
export class AppComponent implements OnInit {
    constructor(private router: Router) {
        this.router
            .events
            .pipe(filter(event => event instanceof NavigationEnd)) // Filtering for certain EventTypes
            .subscribe(console.log);
    }

    ngOnInit() {
    }
}
```

## using jest with angular and angular-cli
### installation
```bash
npm i --save-dev  jest jest-preset-angular
```
### config
```json package.json
  ...

  "scripts": {
    "test": "jest --coverage",
  },

  ...

  "jest": {
    "preset": "jest-preset-angular",
    "roots": [
      "src"
    ],
    "setupTestFrameworkScriptFile": "<rootDir>/src/setup.jest.ts",
    "transformIgnorePatterns": ["node_modules/(?!@ngrx)"],
    "coveragePathIgnorePatterns": ["setup.jest.ts"]
  }

```
```js setup.jest.ts
import 'jest-preset-angular';

Object.defineProperty(document.body.style, 'transform', {
    value: () => {
        return {
            enumerable: true,
            configurable: true,
        };
    },
});
```

### removing all auto-generated spec files
* if you want to have a fresh start with jest and have not yet written any tests, you can remove all **.spec.ts** files generated by the cli with the following powershell command. **Make sure to run this within the src folder, not above**
```powershell
ls *.spec.ts -Recurse # To list the file before running the delete command
ls *.spec.ts -Recurse | foreach {rm $_} # remove the files
```