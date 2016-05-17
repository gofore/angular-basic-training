# Angular 2 Fundamentals
- Background
- Architecture
- Components
- Templates
- Component Lifecycle Hooks
- Two-way Data Binding
- Services
- Asynchronous and Server-side Communication

---

# Background
- Based on successful Angular 1
- First beta on December 2015
- First release candidate on April 2016
- Not even considered ready by features, not to mention bugs

---

# Angular 2 & TypeScript
- Available for JavaScript, Dart and TypeScript
- TypeScript as de facto standard and recommended by many A2 core teams
- Framework code under `@angular` packages such as `@angular/core` and `@angular/common`

---

# Architecture
- Tree structure of components
  - Each component has a template in which more components can be used
  - Components can also use services (singletons) to e.g. share data

![Component tree](angular2-fundamentals/component-tree.png "Component tree")

---

# File Structure

```
project
│  package.json
└──src
    │  app.component.ts
    │  index.html
    ├──videos
    │   └──components
    │       │   videos.component.html
    │       │   videos.component.ts
    │       │   video.component.html
    │       │   video.component.ts
    │   └──services
    │       │   video.service.ts
    │   └──pipes
    └──images
```

---

# Components
- Defines a build block for application UI
- Has a template that it binds data to
- Can contain other components

---

# Usage
Class with `@Component` annotation

_videos.component.ts_
```typescript
@Component({})
class VideosComponent {
}
```

---

# Usage
Template with `templateUrl`

_videos.component.ts_
```typescript
@Component({
  `templateUrl: 'videos.component.html'`
})
class VideosComponent {
}
```

_videos.component.html_
```html
<div>
  <h1>My videos</h1>
</div>
```

---

# Usage
Selector for usage in templates

_videos.component.ts_
```typescript
@Component({
  templateUrl: 'videos.component.html',
  `selector: 'videos'`
})
class VideosComponent {
}
```

_app.component.ts_
```typescript
*import {VideoComponent} from './video.component';
@Component({
* providers: [VideoComponent]
})
class AppComponent {
}
```

_app.component.html_
```html
<videos></videos>
```

---

# Usage
Data binding

_videos.component.ts_
```typescript
@Component({
  selector: 'videos',
  templateUrl: 'videos.component.html'
})
class VideosComponent {
  `private videoName: string = 'Superman';`
}
```

_videos.component.html_
```html
<div>
  <h1>`{{videoName}}`</h1>
</div>
```

---
# Usage
Iterating over array

_videos.component.ts_
```typescript
@Component({
  selector: 'videos',
  templateUrl: 'videos.component.html'
})
class VideosComponent {
  `private videos: any[] = [{name: 'Batman'}, {name: 'Superman'}];`
}
```

_videos.component.html_
```html
<div `*ngFor="let video of videos"`>
  {{video.name}}
</div>
```

---

# Usage
Event handlers

_videos.component.html_
```html
<div *ngFor="let video of videos">
  <div `(click)="openVideo(video)"`>{{video.name}}</div>
</div>
```

_videos.component.ts_
```typescript
@Component({
  selector: 'videos',
  templateUrl: 'videos.component.html'
})
class VideosComponent {
  videos: Video[] = [{name: 'Batman'}, {name: 'Superman'}];  

* openVideo(video: Video) {
*   // Open video
* }
}
```

---
# Usage
Inputs

_videos.component.html_
```html
<div *ngFor="let video of videos">
  <video `[data]="video"`></video>
</div>
```

_video.component.ts_
```typescript
@Component({
  selector: 'video'
})
class VideoComponent {
* @Input() data: Video;
}
```


---

# Usage
Outputs

_video.component.ts_
```typescript
@Component({
  selector: 'video',
  templateUrl: 'video.component.html'
})
class VideoComponent {
* @Output() rate: EventEmitter<number>;

* rate(rating: number) {
*   this.rate.emit(rating);
* }
}
```

_videos.component.html_
```html
<div *ngFor="let video of videos">
  <video `(rate)="currentRating = $event"`></video>
  {{currentRating}}
</div>
```

---

# Usage
Dependency injection with constructor parameters

_videos.component.ts_
```typescript
*import {BackendService} from './backend.service';

@Component({
  selector: 'videos',
  templateUrl: 'videos.component.html'
})
class VideosComponent {
* private backendService: BackendService;
* constructor(backendService: BackendService) {
*   this.backendService = backendService;
* }
}
```

---

# Usage
Visibility

_videos.component.ts_
```typescript
import {BackendService} from './backend.service';

@Component({
  selector: 'videos',
  templateUrl: 'videos.component.html'
})
class VideosComponent {
  constructor(`private` backendService: BackendService) {}
}
```

---

# Usage
Inline styles

_videos.component.ts_
```typescript
@Component({
  selector: 'videos',
  templateUrl: 'videos.component.html',
  `styles: ['.good-video { background-color: green; }']`
})
class VideosComponent {
  constructor() {}
}
```

Styles only visible within this component (_videos.component.html_)

---

# Usage
Styles from URL

_videos.component.ts_
```typescript
@Component({
  selector: 'videos',
  templateUrl: 'videos.component.html',
  `styleUrls: ['./videos.component.css']`
})
class VideosComponent {
  constructor() {}
}
```

---

# Component-template bindings
- There are different types of interaction between the component and template
  - Bind data to be shown with `{{property name}}`
  - Attributes with `[attribute name]`
  - Event handlers with `(event name)`
  - Templates with `*template name`


_videos.component.html_

```html
<h2>Video `{{name}}`</h2>
<div `*ngFor`="let video of videos">
  <video `[data]`="video" `(click)`="openVideo(video)"></video>
</div>
```

---
# Bootstrapping the Application

_index.ts_
```typescript
import {AppComponent} from './app.component';
bootstrap(AppComponent, [UserService, BackendService]);
```
---

# Exercise 1.1

---

# Component Lifecycle Hooks
- For hooking into certain lifecycle events
- Interface for each hook
- Example hooks: `ngOnInit`, `ngOnChanges` and `ngOnDestroy`

```typescript
export class MyComponent `implements OnInit` {
  `ngOnInit() { ... }`
}
```

---

# Two-way Data Binding

- Combining the `()` (output) and `[]` (input) as `([])` gives us two-way data binding
- `ngModel` allows us to bind input field into components property two-way:

```html
<label>Your name: <input type="text" `[(ngModel)]="name"` /></label>
```

---

# Exercise 1.2

---

# Services

- Application-wide singletons
- Used to store state, communicate with backends etc.
- Examples:
  - `UserService`
  - `BackendService`

---
# Usage
`@Injectable` annotation

_user.service.ts_
```typescript
*@Injectable()
export class UserService {
}
```

---
# Usage
Include other services

_user.service.ts_
```typescript
@Injectable()
export class UserService {
  constructor(`backendService: BackendService`) {
  }
}
```
---

```typescript
*import {UserService} from './user.service';

@Component({...})
export class MyComponent {
  private role:
  constructor(`private userService: UserService`) {
    this.role = userService.getRole();
  }
}
```

---

# Exercise 1.3

---

# Asynchronous and Server-side Communication

- Asynchronous is managed in Angular 2 by Observables (covered on advanced topics)
  - For now, we only need to know that there is `subscribe` method
- For AJAX requests, there is `Http` service with support for GET, POST, PUT, DELETE, HEAD and PATCH requests

```typescript
@Component({...})
export class MyComponent {
  private filteredData: any[];
  constructor(private http: Http) {
  }

  getData() {
*    this.http.get('https://example.com/mydata').subscribe(data => {
      // Do stuff with data
      this.filteredData = data.filter(item => item.id > 100);
    })
  }
}
```

---

# Exercise 1.4
