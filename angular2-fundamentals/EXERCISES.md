# Exercise 1 - Todo List

Start by cloning the exercise seed, if not done yet:

```bash
git clone https://github.com/gofore/angular2-seed angular2-exercise
cd angular2-exercise
npm install -g webpack webpack-dev-server typings typescript
npm install
npm start
```

## 1.1 Components and Templates
A simple todo list.

Tasks:
- List todo items from _items_ property

```typescript
private items = [{name: 'Do the laundry', done: false}, {name: 'Clean my room', done: false}];
```

like

| Task           | Done  |
|----------------|-------|
| Do the laundry | false |
| Clean my room  | false |

- Mark items done/undone by clicking them
- Bonus: Add strikethrough to item names with done status

## 1.2 Two-way Data Binding

- Add new items
- Add a check box for marking items done
- Bonus: Disable add item button if item name is empty

## 1.3 Service for Todo Items

- Implement a todo service and inject it to your todo list component
- Service should be used for getting the list of todo items

## 1.4 REST API

- Modify your todo service to use the given [Todo REST API](https://github.com/gofore/todo-backend) for the following functions:
  - List items
  - Add items
  - Change item done status
- Bonus: Add removal of items
- Bonus: Sort todo list by item id
