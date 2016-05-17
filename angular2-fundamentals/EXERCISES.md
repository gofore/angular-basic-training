# Exercise 1 - Todo List

Your first exercise is to implement a todo list application with Angular 2. Start by cloning the exercise seed:
```bash
git clone https://github.com/gofore/angular2-seed angular2-exercise
npm install
npm start
```

## 1.1 Components and Templates
Implement a todo list application:
- Technical requirements:
  - Todo list and todo list item should be separate components
- Specification:
  - List todo items came from a static array:

    ```typescript
    items: [{name: 'Do the laundry', done: false}, {name: 'Clean my room', done: false}];
    ```

  - Mark items done by clicking them (show the item done-status after the item name)
  - Bonus task: Add strikethrough to item names with done status

## 1.2 Two-way Data Binding

- Add new items
- Add a check box for marking items done
- Bonus task: Disable add item button if item name is empty

## 1.3 Service for Todo Items

- Implement a todo service and inject it to your todo list component
- Service should be used for getting the list of todo items

## 1.4 REST API

- Modify your todo service to use the given [Todo REST API](https://github.com/gofore/todo-backend) for the following functions:
  - List items
  - Add items
  - Change item done status
- Bonus task: Add removal of items
- Bonus task: Sort todo list by item id
