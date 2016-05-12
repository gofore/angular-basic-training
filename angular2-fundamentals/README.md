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

---

# Architecture
- Tree structure of components
  - Each component has a template in which more components can be used
  - Components can also use services

![Component tree](angular2-fundamentals/component-tree.png "Component tree")

---

# Components
- Defines a build block for application UI
- Binds data to template
- Has an unique selector for usage as part of other components
- Has well-defined inputs and outputs

---

_videos.component.ts_

```typescript
@Component({
  selector: 'videos',
  templateUrl: 'videos.component.html'
})
class VideosComponent {
  @Input() videos: Video[];
  @Output() rate: EventEmitter<number>;

  videoClicked(video: Video) {
    // Handle click
  }
}
```

---

# Templates

- Template is always bound to component
- Can declare different type of interactions with the component
  - Bind data to be shown with `{{property name}}`
  - Attributes with `[attribute name]`
  - Event handlers with `(event name)`
  - Templates with `*template name`

_videos.component.html_

```html
<h2>Video `{{name}}`</h2>
<div `*ngFor`="let video of videos">
  <video `[video]`="video" `(click)`="videoClicked($event)"></video>
</div>
```

---

_video.component.ts_
```typescript
@Component({
  selector: 'video',
  templateUrl: 'video.component.html'
})
class VideoComponent {
  @Input() video: Video;
  @Output() rate: EventEmitter<number>;

  rate(rating: number) {
    this.rate.emit(rating);
  }
}
```


_video.component.html_
```html
  <span
    (click)="videoClicked($event)"
    [class.disabled]="video.disabled">
    {{video.name}}
  </span>
```

---

# Component Lifecycle Hooks
- Components have set of lifecycle hooks that they can register handlers for
- Each hook is introduced by specific interface that can be implemented
- Example hooks: `ngOnInit`, `ngOnChanges` and `ngOnDestroy`

```typescript
export class MyComponent `implements OnInit` {
  `ngOnInit() { ... }`
}
```

---

# Two-way Data Binding

- Combining the `()` and `[]` as `([])` gives us two-way data binding
- Giving `ngModel` allows us to bind input field into components property two-way:

```html
<label>Your name: <input type="text" `[(ngModel)]`="name" /></label>
```

- If the value of `name` is changed in our code, it updates to view. If the user types something on the input, the `name` attribute is updated accordingly

---

# Services

- Application-wide singletons (we'll come back to this later)
- Can be injected to any component
- Examples:
  - `UserService`
  - `BackendService`

---

# Example - User service
_user.service.ts_

```typescript
@Injectable()
export class UserService {
  private userRequest: Observable<User>;
  constructor(backendService: BackendService) {
    this.userRequest = backendService.fetchUser();
  }

  getRole() {
    return this.userRequest.map(user => user.role);
  }
}
```
which could now be used in component as follows
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

# Asynchronous and Server-side Communication

- Asynchronous is managed in Angular 2 by Observables (covered on advanced topics)
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
