# Testing

---

# Unit testing
- Testing of components in isolation from other components
- Guard against changes that break existing code
- Specify and clarify what the code does
- Can be executed automatically e.g. on file save (concept of _guarding_)

---
# Jasmine
- Unit testing framework for JavaScript
- Simple basic syntax:
  - `describe(string, function)` to define suite of test cases
  - `it(string, function)` to declare single test case
  - `expect(a).toBe(b)` to make assertions
  - If all assertions are true, test case succeeds
  - Also a large number of different kind of assertions such as `toBeDefined()`, `toMatch(regex|string)`, `toContain(regex|string)` and `toBeGreaterThan(number)`

# First Jasmine test

```javascript
it('true is true', () => expect(true).toEqual(true););
```

---
# Setup and tear-down
- To keep test DRY, you can use `beforeEach`, `afterEach`, `beforeAll` and `afterAll`
```javascript
describe("A spec using beforeEach and afterEach", () => {
      var foo = 0;

      beforeEach(() => {
        foo += 1;
      });

      afterEach(() => {
        foo = 0;
      });

      it("is just a function, so it can contain any code", () => {
        expect(foo).toEqual(1);
      });

      it("can have more than one expectation", () => {
        expect(foo).toEqual(1);
        expect(true).toEqual(true);
      });
});
```

---
# Spies
- Test double functions AKA spies let you stub any function and track calls to it
```javascript
describe("A spy", () => {
      var foo, bar = null;
      beforeEach(() => {
        foo = {
          setBar: (value) => {
            bar = value;
          }
        };
        spyOn(foo, 'setBar');
        foo.setBar(123);
        foo.setBar(456, 'another param');
      });

      it("tracks that the spy was called", () => {
        expect(foo.setBar).toHaveBeenCalled();
      });
      it("tracks that the spy was called x times", () => {
        expect(foo.setBar).toHaveBeenCalledTimes(2);
      });
      it("tracks all the arguments of its calls", () => {
        expect(foo.setBar).toHaveBeenCalledWith(123);
        expect(foo.setBar).toHaveBeenCalledWith(456, 'another param');
      });
});
```
---
# Angular Testing Platform (ATP)
- Angular 2 has its own testing platform that makes testing Angular components a lot easier
- Enables developers to write isolated unit tests
  - For services, components, directives, pipes
  - Provides tools to fake all dependencies and injected values
- Consists of TestBed class and some helper functions
- Provides tools to test components and services that have async behavior
---
# Example spec
- Suppose we had the following component:
```typescript
    @Component({
      selector: 'videos',
      templateUrl: 'videos.component.html'
    })
    class VideosComponent {
      private videos: Video[];
      constructor(videoService: VideoService) {
        this.videos = videoService.getList();
      }
    }
```
---
# Setup
- We can setup the test environment with mocks using TestBed:
```typescript
    beforeEach(() => {
      TestBed.configureTestingModule({
        declarations: [ VideosComponent ],
        providers: [{provide: VideoService, useClass: FakeVideoService}]
      });

      // create component and test fixture
      fixture = TestBed.createComponent(VideosComponent);

      // get test component from the fixture
      comp = fixture.componentInstance;
    });
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
- Angular Testing Platform provides two different approaches for managing this
---
# async()
- We can run test code within asynchronous zone
```typescript
    it('should show videos after getVideos promise (async)', async(() => {
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
    it('should show videos after getVideos promise (fakeAsync)', fakeAsync(() => {
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
ng test
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
  ng e2e
  ```
---
# Example spec
```javascript
describe('angularjs homepage todo list', () => {
  it('should add a todo', () => {
    browser.get('https://angularjs.org');

    element(by.model('todoList.todoText')).sendKeys('write first protractor test');
    element(by.css('[value="add"]')).click();

    var todoList = element.all(by.repeater('todo in todoList.todos'));
    expect(todoList.count()).toEqual(3);
    expect(todoList.get(2).getText()).toEqual('write first protractor test');

    // You wrote your first test, cross it off the list
    todoList.get(2).element(by.css('input')).click();
    var completedAmount = element.all(by.css('.done-true'));
    expect(completedAmount.count()).toEqual(2);
  });
});
```
