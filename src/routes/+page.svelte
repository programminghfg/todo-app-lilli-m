<script>
  import { onMount } from 'svelte';
  import { browser } from '$app/environment';
  
s
  let todoText = '';
  let todos = [];

  onMount(() => {
    if (browser) {
      const storedTodos = JSON.parse(localStorage.getItem('todos'));
      if (storedTodos) {
        todos = storedTodos;
      }
    }
  });

  function saveTodos() {
    if (browser) {
      localStorage.setItem('todos', JSON.stringify(todos));
    }
  }

  function addTodo() {
    todos.push({ text: todoText, done: false });
    todos = todos;
    todoText = '';
    saveTodos();
  }

  function remove(index) {
    todos.splice(index, 1);
    todos = todos;
    saveTodos();
  }
</script>

<style>
  h1 {
    font-size: 24px;
    font-weight: bold;
    text-align: center;
    margin-bottom: 20px;
  }

  
  .todo-input {
    width: 100%;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-bottom: 10px;
    transition: border-color 0.3s ease; /* Add this line */
  }

  .todo-input:hover {
    border-color: #888; /* Add this block */
  }

  .add-button {
    display: block;
    width: 100%;
    padding: 10px;
    font-size: 16px;
    background-color: #7845a0;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .add-button:hover {
    background-color: #7845a0;
  }

  .todo-entry {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
    text-decoration: none; /* Add this line */
  }
  .todo-entry.done {
    text-decoration: line-through; /* Add this block */
  }
  .todo-text {
    flex: 1;
    font-size: 16px;
    margin-right: 10px;
  }

  .todo-done {
    margin-right: 10px;
  }

  .delete-button {
    background: none;
    border: none;
    color: #f44336;
    font-size: 16px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: color 0.3s ease;
    position: relative;
    overflow: hidden;
    width: 24px;
    height: 24px;
    border-radius: 50%;
  }

  .delete-button:hover {
    color: #ff0000;
  }

  .delete-button::before {
    content: "";
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #ccc;
    opacity: 0;
    transition: opacity 0.3s ease;
  }

  .delete-button:hover::before {
    opacity: 1;
  }
</style>



<h1>TODO APP</h1>

<input type="text" class="todo-input" bind:value={todoText} />
<button class="add-button" on:click={addTodo}>ADD</button>

{#each todos as todo, index}
  <div class="todo-entry" class:done={todo.done}>
    <input
      type="checkbox"
      class="todo-done"
      bind:checked={todo.done}
      id="checkbox_{index}"
    />
    <label for="checkbox_{index}" class="todo-text">{todo.text}</label>
    <button
      class="delete-button"
      on:click={() => {
        remove(index);
      }}
    >
      X
    </button>
  </div>
{/each}