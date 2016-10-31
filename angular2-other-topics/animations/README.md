# Animations
- Built on top of Web Animations specification
- Part of `@angular/core` module

---

# Browser Support
![Web Animations compatibility table](angular2-other-topics/animations/web-animations-compatibility.png "Web Animations compatibility table")
[Polyfill](https://github.com/web-animations/web-animations-js) for older browsers

---

# Adding Polyfill to Angular CLI

```shell
npm install --save web-animations-js
```

_angular-cli.json_
```json
"scripts": [
  "../node_modules/web-animations-js/web-animations.min.js"
],
```

---

# Animations - Supported Properties
- List available in [the standard](https://www.w3.org/TR/css3-transitions/#animatable-properties)
- Basically every color (background, border, etc.) and dimension (width, height, size, etc.)
  - `background-color`
  - `border-width`
  - `max-height`

---

# Animations API
- Part of `@angular/core`:
  - `trigger`
  - `state`
  - `style`
  - `transition`
  - `animate`
- Declared within the `@Component` annotation:

```typescript
@Component({
  animations: [
    trigger(...)
  ]
})

```

---

# Animation States
- _Trigger_ is bound to the certain element
- _States_ are the possible values for triggered value
- States have _styles_
- _Transitions_ between states are animated
- _Animations_ have duration and timing functions etc.

---

# Triggers
_Trigger_ is bound to the certain element

_my.component.ts_
```typescript
@Component({
  animations: [
    trigger('myTrigger', [
      ...
    ])
  ]
})
export class MyComponent {
  state = 'state1';
}
```

_my.component.html_
```html
<div [@myTrigger]="state">
  My element
</div>
```

---

# States
_States_ are the possible values for triggered value

_my.component.ts_
```typescript
trigger('myTrigger', [
*  state('state1', ...),
*  state('state2', ...)
])
export class MyComponent {
  state = 'state1';

  changeState() {
    state = 'state2';
  }
}
```

---

# Styles
States have _styles_

_my.component.ts_
```typescript
trigger('myTrigger', [
  state('state1', style({
    'backgroundColor': '#fff'
  })),
  state('state2', style({
    'backgroundColor': '#000'
  }))
])
export class MyComponent {
  state = 'state1';

  changeState() {
    state = 'state2';
  }
}
```

---

# Transitions
_Transitions_ between states are animated

_my.component.ts_
```typescript
trigger('myTrigger', [
  state(...),
* transition('state1 => state2', ...),
* transition('state2 => state1', ...)
])
export class MyComponent {
  state = 'state1';

  changeState() {
    state = 'state2';
  }
}
```

---

# Animations
_Animations_ have duration and timing functions etc.

_my.component.ts_
```typescript
trigger('myTrigger', [
  state(...),
  transition('state1 => state2', `animate('100ms ease-in')`),
  transition('state2 => state1', `animate('100ms ease-out')`)
])
export class MyComponent {
  state = 'state1';

  changeState() {
    state = 'state2';
  }
}
```

---

# Special States
- Two special states:
  - `void`: element is not attached to a view
  - `*`: matches any animation state
- Can be used in transitions:
  - `transition('void => *', ...)`: element enters the view
  - `transition('* => void', ...)`: element leaves the view
- Entering and leaving also have aliases:
```typescript
transition(`':enter'`, ...); // void => *
transition(`':leave'`, ...); // * => void
```
