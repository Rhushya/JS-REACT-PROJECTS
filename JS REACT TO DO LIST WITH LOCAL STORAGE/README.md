# React Todo List App

This is a simple React Todo List application that allows you to add, mark as complete, and clear todos. The application also attempts to store the todos in the browser's local storage.

## Features

- Add new todos
- Mark todos as complete
- Clear all todos
- Persistent storage using local storage

## Installation

To run this project locally, follow these steps:

1. **Clone the repository:**
2. **cd..  into the directory of the file **
3. **npm start **

Usage
Add a Todo:

Type a todo item into the input field and click "Add Todo".
Mark as Complete:

Click the checkbox next to a todo item to mark it as complete.
Clear Todos:

Click the "Clear Todos" button to remove all todos from the list.
Known Issues
Local Storage
Currently, there is an issue with the local storage functionality. The todos are not being saved or retrieved correctly. If you know how to fix this issue, please consider contributing to this project.

Contributing
Contributions are welcome! If you have any suggestions or improvements, please submit a pull request or open an issue.



### Complete Code: `App.js`

```javascript
import React, { useState, useRef, useEffect } from "react";
import { v4 as uuidv4 } from "uuid";
import "./App.css";

const LOCAL_STORAGE_KEY = "todoApp.todos";

function App() {
  const [todos, setTodos] = useState([]);
  const todoNameRef = useRef();

  // Load todos from localStorage on component mount
  useEffect(() => {
    const storedTodos = JSON.parse(localStorage.getItem(LOCAL_STORAGE_KEY));
    if (storedTodos) setTodos(storedTodos);
  }, []);

  // Save todos to localStorage whenever the todos state changes
  useEffect(() => {
    localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(todos));
  }, [todos]);

  // Add a new todo
  function handleAddTodo() {
    const name = todoNameRef.current.value;
    if (name === "") return;
    setTodos((prevTodos) => {
      return [...prevTodos, { id: uuidv4(), name: name, complete: false }];
    });
    todoNameRef.current.value = null;
  }

  // Clear all todos
  function handleClearTodos() {
    setTodos([]);
  }

  // Toggle todo completion
  function toggleTodo(id) {
    const newTodos = [...todos];
    const todo = newTodos.find((todo) => todo.id === id);
    todo.complete = !todo.complete;
    setTodos(newTodos);
  }

  return (
    <div className="app-container">
      <h1>Todo List</h1>
      <TodoList todos={todos} toggleTodo={toggleTodo} />
      <input ref={todoNameRef} type="text" placeholder="Enter todo" />
      <div>
        <button onClick={handleAddTodo}>Add Todo</button>
        <button onClick={handleClearTodos}>Clear Todos</button>
      </div>
      <footer>
        <div>{todos.filter((todo) => !todo.complete).length} left to do</div>
      </footer>
    </div>
  );
}

function TodoList({ todos, toggleTodo }) {
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id} className={todo.complete ? "complete" : ""}>
          <label>
            <input
              type="checkbox"
              checked={todo.complete}
              onChange={() => toggleTodo(todo.id)}
            />
            {todo.name}
          </label>
        </li>
      ))}
    </ul>
  );
}

export default App;

```

### Complete Code: `App.css`
```
body {
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.app-container {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  width: 300px;
  max-width: 90%;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
  text-align: center;
}

input[type="text"] {
  width: calc(100% - 22px);
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  width: calc(50% - 12px);
  padding: 10px;
  margin: 5px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

button:disabled {
  background-color: #ccc;
}

button:hover {
  background-color: #0056b3;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid #ddd;
}

li:last-child {
  border-bottom: none;
}

input[type="checkbox"] {
  margin-right: 10px;
}

.complete {
  text-decoration: line-through;
  color: #aaa;
}

footer {
  margin-top: 10px;
  text-align: center;
}
```
How to Use
Save the README.md content to a file named README.md.
Save the App.js content to a file named src/App.js in your React project.
Save the App.css content to a file named src/App.css in your React project.


