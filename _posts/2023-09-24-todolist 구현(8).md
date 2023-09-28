---
title: TodoList 구현해보기 (8) - Todo CRUD
author: hyebeen
date: 2023-09-24 16:32:00 +0800
categories: [React, TodoList]
tags: [react]
image: /assets/img/posts/react.jpeg
---

# 간단한 Todo CRUD(Create, Read, Update, Delete) 기능

## LocalTodoService 클래스

- 로컬 스토리지를 사용하여 할 일(todo) 목록을 관리하는 기능

```js
export class LocalTodoService {
  constructor(todoStorage) {
    this.id = 0;
    this.todos = todoStorage.get() || [];
    this.todoStorage = todoStorage;
  }

  async get() {
    return this.todos;
  }

  async create(todo) {
    const newTodo = { id: ++this.id, todo, isComplete: false, userId: 1 };
    const newTodos = [...this.todos, newTodo];

    this.todos = newTodos;
    this.todoStorage.save(this.todos);

    return newTodo;
  }

  async update(id, updatedTodo) {
    let todos = this.todos.map((todo) =>
      todo.id === id ? { ...todo, ...updatedTodo } : todo
    );

    this.todoStorage.save(todos);
    this.todos = todos;
  }

  async delete(id) {
    let todos = this.todos.filter((todo) => todo.id !== id);

    this.todoStorage.save(todos);
    this.todos = todos;
  }
}
```

### 1. 생성자(Constructor)

- 초기 id를 0으로 설정
- todoStorage로부터 가져온 할 일 목록을 this.todos에 저장
- 만약 todoStorage.get()이 빈 값을 반환한다면, this.todos는 빈 배열([])로 설정

```js
constructor(todoStorage) {
  this.id = 0;
  this.todos = todoStorage.get() || [];
  this.todoStorage = todoStorage;
}

```

### CRUD(get,create,update,delete)

- get 메서드
  - 현재 저장된 모든 할 일들을 반환

```js
async get() {
  return this.todos;
}

```

- create 메서드
  - 새로운 할 일을 생성하고 이를 현재의 할 일 목록에 추가한 후, 변경된 목록을 스토리지에 저장

```js
async create(todo) {
  const newTodo = { id: ++this.id, todo, isComplete: false, userId: 1 };
  const newTodos = [...this.todos, newTodo];

  this.todos = newTodos;
  this.todoStorage.save(this.todos);

  return newTodo;
}

```

- update 메서드
  - 주어진 id와 매칭되는 할 일의 속성들을 업데이트하고 변경된 목록을 스토리지에 저장

```js
async update(id, updatedTodo) {
  let todos = this.todos.map((todo) =>
    todo.id === id ? { ...todo, ...updatedTodo } : todo
  );

  this.todoStorage.save(todos);
  this.todos = todos;
}

```

- delete 메서드
  - 주어진 id와 매칭되는 할 일을 삭제하고 변경된 목록을 스토리지에 저장

```js
async delete(id) {
  let todos = this.todos.filter((todo) => todo.id !== id);

  this.todoStorage.save(todos);
  this.todos = todos;
}

```

---

## TodoStorage 클래스

- localStorage를 사용하여 할 일(todo) 목록을 관리하는 기능

```js
export class TodoStorage {
  #todo;

  constructor() {
    // 'todos'라는 키를 this.#todo에 저장
    this.#todo = "todos";
  }

  save(todos) {
    localStorage.setItem(this.#todo, JSON.stringify(todos));
  }

  get() {
    const storedTodos = localStorage.getItem(this.#todo);
    return storedTodos ? JSON.parse(storedTodos) : [];
  }

  delete() {
    localStorage.removeItem(this.#todo);
  }
}
```

- save 메서드
  - 주어진 할 일 목록을 JSON 형식으로 변환한 후, this.#todo라는 키로 localStorage에 저장
- get 메서드
  - this.#todo라는 키로 localStorage에서 데이터를 가져옵니다.
  - 가져온 데이터가 있으면 이를 객체 배열로 변환하여 반환하고, 그렇지 않으면 빈 배열([])을 반환
- delete 메서드
  - this.#todo라는 키로 저장된 항목을 localStorage에서 삭제
