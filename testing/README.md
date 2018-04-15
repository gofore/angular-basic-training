# Testing

---

# Unit testing
- Testing of components in isolation from other components
- Guard against changes that break existing code
- Specify and clarify what the code does

---

# Jasmine
- Unit testing framework for JavaScript
- Simple basic syntax:
  - `describe(string, function)` to define suite of test cases
  - `it(string, function)` to declare single test case
  - `expect(a).toBe(b)` to make assertions
  
---

# First Jasmine Test
- Class under test
```typescript
  export class Person {
      constructor(private name: string) {
      }

      getName() {
        return this.name;
      }
  }
```

---

# First Jasmine Test
- Test case
```typescript
  describe('Person', () => {
      let person;

      beforeEach(() => {
        person = new Person('John');
      });

      it('should return name', () => {
        expect(person.getName()).toBe('John');
      });
  });
```

---

# Spies
- Test double functions AKA spies let you stub any function and track calls to it

```typescript
it('tracks that the spy was called', () => {
  spyOn(person, 'getName');

  person.getName();
  expect(person.getName).toHaveBeenCalled();
});
```

---

# Spies
```typescript
it('stubs the function return value', () => {
  spyOn(person, 'getName').and.returnValue('Jane');

  expect(person.getName()).toBe('Jane');
});
```

---

# Angular Testing Platform (ATP)
- TestBed for wiring angular components for testing
- Inject mock dependencies
- Testing components with async behavior

---

# Component Under Test
- Suppose we had the following component:
```typescript
    @Component({
      selector: 'videos',
      templateUrl: 'videos.component.html'
    })
    class VideosComponent {
      videos: Video[];

      constructor(videoService: VideoService) {
        this.videos = videoService.getList();
      }
    }
```

---

# Wiring Up
- We can setup the test environment with mocks using TestBed:
```typescript
    beforeEach(() => {
      TestBed.configureTestingModule({
        declarations: [ VideosComponent ],
        providers: [{provide: VideoService, useClass: FakeVideoService}]
      });
    });
```

---

# Access to Tested Component
```typescript
fixture = TestBed.createComponent(VideosComponent);

comp = fixture.componentInstance;

debugElement = fixture.debugElement;

nativeElement = fixture.nativeElement;

```

---

# Async with ATP
- Let's change the implementation of the VideoService to return a promise:
```typescript
    ngOnInit() {
      this.videoService.getList().then(videos => this.videos = videos);
    }
```
- This means that we'll have to deal with asynchronous behavior

---

# async()
- We can run test code within asynchronous zone
```typescript
    it('should show videos', async(() => {
      fixture.detectChanges();          // trigger data binding
      fixture.whenStable().then(() => { // wait for async getVideos
        fixture.detectChanges();        // update view with videos
        expect(getVideos()).toBe(testVideos);
      });
    }));
```

---

# fakeAsync()
- Or within fake asynchronous zone
```typescript
    it('should show videos', fakeAsync(() => {
      fixture.detectChanges(); // trigger data binding
      tick();                  // wait for async getVideos
      fixture.detectChanges(); // update view with videos
      expect(getVideos()).toBe(testVideos);
    }));
```

---

# Karma
- Karma is a test runner with support e.g. for coverage reports and test results exports
- Angular CLI comes with Karma installed
- To run tests, type:
```shell
npm run test
```

---

# E2E testing
- End-to-End (E2E) tests test flow of the application
- Ensures that the components of the application function together as expected
- E2E tests often define use cases of the application

---

# Protractor
- E2E test framework for Angular applications
- Runs tests against your application running in a real browser, interacting with it as a user would
- Angular CLI comes with Protractor installed
  - To run E2E tests, type:
  ```shell
  npm run e2e
  ```

---

# Example spec
```javascript
describe('Protractor Demo App', () => {
  it('should have a title', () => {
    browser.get('http://demo.com/protractor-demo/');

    expect(browser.getTitle()).toEqual('Demo Application');
    expect(element(by.css('h1')).toEqual('Header');
  });
});
```
