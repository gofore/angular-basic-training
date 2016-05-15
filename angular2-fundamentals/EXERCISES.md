# Exercise 2 - Todo List

Your first exercise is to implement a todo list application with Angular 2

## 2.1 Components and Templates

Implement a todo list application with the following features for the user:
- List todo items in the template from a static array:

```typescript
items: [{name: 'Do the laundry', done: false}, {name: 'Clean my room', done: false}];
```

- Mark items done by clicking them (show the item done-status after the item name)

## 2.2 Two-way Data Binding

- Add new items
- Add a check box for marking items done
- Bonus task: Add strikethrough to item names with done status

## 2.3 Service for Todo Items

- Implement a todo service and inject it to your todo list component
- Service should be used for getting the list of todo items

## 2.4 REST API

- Modify your todo service to use the given REST API
- Bonus task: Add removal of items
