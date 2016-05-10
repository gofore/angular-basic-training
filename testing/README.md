# Testing

---
# Unit testing
- Testing of components in isolation from other components
- Usually done automatically on file save (concept of _guarding_)

---
# Jasmine
- Unit testing tool for JavaScript
- Simple basic syntax:
  - `describe(string, function)` to define suite of test cases
  - `it(string, function)` to declare single test case
  - `expect(a).toBe(b)` to make assertions
  - If all assertions are true, test case succeeds
  - Also a large number of different kind of assertions such as `toBeDefined()`, `toMatch(regex|string)`, `toContain(regex|string)` and `toBeGreaterThan(number)`

# First Jasmine test
```html
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8">
  <title>Ng App Unit Tests</title>
  <link rel="stylesheet" href="../node_modules/jasmine-core/lib/jasmine-core/jasmine.css">
  <script src="../node_modules/jasmine-core/lib/jasmine-core/jasmine.js"></script>
  <script src="../node_modules/jasmine-core/lib/jasmine-core/jasmine-html.js"></script>
  <script src="../node_modules/jasmine-core/lib/jasmine-core/boot.js"></script>
</head>
<body>
  <script>
    it('true is true', function(){ expect(true).toEqual(true); });
  </script>
</body>
</html>
```
---
# Setup and tear-down
- To keep test DRY, you can use `beforeEach`, `afterEach`, `beforeAll` and `afterAll`
```javascript
describe("A spec using beforeEach and afterEach", function() {
  var foo = 0;

  beforeEach(function() {
    foo += 1;
  });

  afterEach(function() {
    foo = 0;
  });

  it("is just a function, so it can contain any code", function() {
    expect(foo).toEqual(1);
  });

  it("can have more than one expectation", function() {
    expect(foo).toEqual(1);
    expect(true).toEqual(true);
  });
});
```

---
# Spies
- Test double functions also known as spies let you stub any function and track calls to it
```javascript
describe("A spy", function() {
  var foo, bar = null;

  beforeEach(function() {
    foo = {
      setBar: function(value) {
        bar = value;
      }
    };

    spyOn(foo, 'setBar');

    foo.setBar(123);
    foo.setBar(456, 'another param');
  });

  it("tracks that the spy was called", function() {
    expect(foo.setBar).toHaveBeenCalled();
  });
  it("tracks that the spy was called x times", function() {
    expect(foo.setBar).toHaveBeenCalledTimes(2);
  });
  it("tracks all the arguments of its calls", function() {
    expect(foo.setBar).toHaveBeenCalledWith(123);
    expect(foo.setBar).toHaveBeenCalledWith(456, 'another param');
  });
  it("stops all execution on a function", function() {
    expect(bar).toBeNull();
  });
});
```
---
# Asynchronous
- Because of the nature of JavaScript, the execution of test cases or setup and tear-down blocks can be asynchronous -> there needs to be a way to let Jasmine know when the block is completely executed
- Jasmine supports this by checking whether there is argument declared for function passed for methods
```javascript
beforeEach(function(done) {
  setTimeout(function() {
    done();
  }, 1000);
});
```
- Default timeout to wait before failing the test case is 5 seconds. Can be changed with extra parameter for `it` call:
```javascript
it("takes a long time", function(done) {
  setTimeout(function() {
    done();
  }, 9000);
}, 10000);
```


---
# Karma
- Using browser for running tests is okay-ish for developing few test cases, but doesn't play well with build automation
- Karma is a test runner with support e.g. for coverage reports and test results exports
- To install Karma, run
```shell
npm install karma karma-jasmine karma-chrome-launcher --save-dev
```
which would allow you to use
```shell
./node_modules/karma/bin/karma start
```
to run tests, which is a little verbose, so run
```shell
npm install -g karma-cli
```
which lets you use just `karma run`

---
# Angular 2 unit testing
- Since Angular 2 code is mostly just ES6 classes it is very easy to instantiate classes for testing. Suppose we had the following component:
```typescript
@Component({
  selector: 'videos',
  templateUrl: 'videos.component.html'
})
class VideosComponent {
}
```
we could instantiate it like
```typescript
new VideosComponent();
```

---
# Mocking
- The way Angular 2 DI is built makes it simple to provide mocked dependencies:
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
```typescript
new VideosComponent({getList: () => [{id: 1, name: 'The video'}]});
```
---
# E2E testing
- End-to-End (E2E) tests the whole flow of application
- Implemented most usually with Protractor

---
# Protractor
- E2E test framework for Angular applications
- Runs tests against your application running in a real browser, interacting with it as a user would
- To install, run
```shell
npm install -g protractor
```
which will install _protractor_ itself and _webdriver-manager_ which can be used to easily boot instance of Selenium Server

---
# Example spec

```javascript
describe('angularjs homepage todo list', function() {
  it('should add a todo', function() {
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

```shell
webdriver-manager update
webdriver-manager start
protractor conf.js
```
