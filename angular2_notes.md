# Angular 2 Notes

## Setup
`<base href="/">` tag if it is a single page app so you can tell Angular where root is.

Third party libraries:
1.  ES6 shims
2.  Systemjs and Angular polyfills for ie.  
3.  Needs SystemJS (can use other things but this recommended by angular team) as file system loader, and must be included in index.html in script tag to load the files we write.  
4.  RxJS must be imported for observables, and http without promises.  
5.  In terms of Angular, we need Dev bundle, Router, Http, and Production bundle.

## Architecture

-Angular 2 is set up to be a component based architecture with a component tree.  The app is bootstrapped and started by passing the angular bootstrap method the main angular app component.  From there, you create component trees, essentially parts of your app, that will come together to form the full app.  The concept of scope from angular 1 is gone, as the context is based on these components and the classes they export.  The app is modularized based on these components and everything lives within there context unless injected explicitly or passed data.


## Components

Components are annotations imported from angular2/core (annotations are Angular 2 specific, decorations are same syntax but we define them with Typescript).

We give the classes metadata with these Component annotations.  Example:

```javascript
@Component ({
  selector: 'my-attribute-directives',
  template: `
  <div myHighlight>
    Highlight me.
  </div>
  <div myHighlight>
    Another Highlight
  </div>
  `,
  directives: [HighlightDirective]
})

export class AttributeDirectives {


}
```

We can also define data handling, with `inputs` and `outputs` in these components, as well as other things.  The class is where much of our logic lives and it is what we are passing around to other components.  

## Data Binding

In Angular 2, two way data binding is no longer the default.  4 ways of binding data:

1.  String Interpolation:  `{{name}}`
2.  Property Binding:  Used when data is flowing from parent to Child.  Uses `[]` syntax.  Binds to elements, can be used as an input in components.
3.  Event Binding:  Used for data to flow from child to parent.  Must bind to an event `()` and then use an `Event-Emitter` (imported from core) and choose what data to `emit()`, and set as output, which can be picked up and reacted to by the parent component.
4.  2 Way Data Binding- -  `[()]` syntax.


## Directives

There are attribute directives and structural directives.  Attribute directives look like HTML directives, but are different.  Example : `[ngClass]`

Attribute directives only bind to elements.  

Structural Directives change the entire page.  Example: `*ngFor` `*ngIf` `*ngSwitch`

Can make own directives using `@Directive` annotation on a class after importing from core.  For directive to take effect on load, must implement class with OnInit lifecycle hook imported from angular core, and use the function that OnInit requires, example with type of any:

```javascript
import {Directive} from 'angular2/core';
import {ElementRef} from 'angular2/core';
import {OnInit} from 'angular2/core'
import {Renderer} from 'angular2/core'

@Directive ({
  selector: '[myHighlight]'
})

export class HighlightDirective implements OnInit {
  private _defaultColor = 'green';

  constructor(private _elRef: ElementRef, private _renderer: Renderer) {}

  ngOnInit():any {
    this._renderer.setElementStyle(this._elRef.nativeElement, 'background-color', this._defaultColor);
  }

}

```

This directive is a custom Attribute Directive modularized in the class `HighlightDirective`, implementing the `OnInit` lifecycle Hook.  It is annotated with a selector that can be used in the elements it affects.  The constructor is a Typescript constructor function that creates a constructor when the class is initiated.  It is injected with a private instance of  `ElementRef` and a private instance of `Renderer`, Angular helper objects, that creates `this._renderer` and `this.elRef` for the class.  You can then use the renderer to set the element style for all Native Elements that the highlight class is attached too.  This directive can be imported and used by any other component within its template with `[]` syntax, as long as it is imported at the top of the file and declared as a directive in the metadata of the component annotation.

## Still Learning:  
How services are handled in Angular 2, routing and http best practices, form validation best practices, pipes, immutable objects and observables and how they affect change detection, zones, providers.
