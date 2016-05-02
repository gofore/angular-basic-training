# Angular 2 fundamentals

---

# Agenda

1. Background
2. Architecture
3. Components
4. Templates
5. Two-way data binding
6. Services
7. Asynchronous and server-side communication

---

# Background

- Based on successful Angular 1
- First beta on December 2015
- First release candidate on April 2016

---

# Architecture

- Tree structure of components

![Component tree](component-tree.png "Component tree")

---

# Components

- Defines a build block for application
- Binds data to template
- Has an unique selector
- Has well-defined inputs and outputs

---
_videos.component.ts_
```typescript
@Component({
  selector: 'x',
  directives: [FormattedRating, WatchButton, RateButton],
  templateUrl: 'videos.component.html'
})
class VideosComponent {
  @Input() videos: Video[];
  @Output() rate: EventEmitter<number>;

  videoClicked(video) {

  }
}
```

# Templates
- Template is always bound to component
- Can declare different type of interactions with the component
  - Bind data to be shown with `{{property name}}`
  - Attributes with `[attribute name]`
  - Event handlers with `(event name)`
  - Templates with `*template name`

_videos.component.html_
```html
<h2>Video {{name}}</h2>
<div *ngFor="let video of videos">
  <span
    (click)="videoClicked($event)"
    [class.disabled]="video.disabled">
    {{video.name}}
  </span>
</div>
```
---

# Two-way data binding
- Combining the `()` and `[]` as `([])` gives us two-way data binding
- Giving `ngModel` allows us to bind input field into components data field two-way:
```html
<label>Your name: <input type="text" ([ngModel])="name" /></label>
```
---

# Services

- Application wide singletons
- Can be injected to any component
- Examples:
  - UserService
  - BackendService

---
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
---

# Asynchronous and server-side communication
- Asynchronous is managed in Angular 2 by Observables (covered on advanced topics)
- For AJAX requests, there is `Http` service with support for GET, POST, PUT, DELETE, HEAD and PATCH requests
---

```typescript
@Component({...})
export class MyComponent {
  private filteredData: any[];
  constructor(private http: Http) {
  }

  getData() {
    this.http.get('https://example.com/mydata').subscribe(data => {
      // Do stuff with data
      this.filteredData = data.filter(item => item.id > 100);
    })
  }
}
```
