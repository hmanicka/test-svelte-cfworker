<script>
  let todos = $state([
    { id: 1, text: 'Learn Svelte 5', done: true },
    { id: 2, text: 'Build an app', done: false }
  ]);
  let new_todo = $state('');
  let next_id = $state(3);
  
  // Computed value for remaining todos
  let remaining = $derived(todos.filter(t => !t.done).length);
  
  function add_todo() {
    if (!new_todo.trim()) return;
    todos = [...todos, { id: next_id, text: new_todo, done: false }];
    next_id++;
    new_todo = '';
  }
  
  function delete_todo(id) {
    todos = todos.filter(t => t.id !== id);
  }
  
  function toggle_todo(id) {
    todos = todos.map(t => {
      if (t.id === id) {
        return { ...t, done: !t.done };
      }
      return t;
    });
  }
  </script>
  
  <main class="todo-app">
    <h1>Todo App</h1>
    
    <form onsubmit={add_todo} class="add-form">
      <input 
        type="text"
        placeholder="What needs to be done?"
        bind:value={new_todo}
        aria-label="New todo text"
      >
      <button type="submit">Add</button>
    </form>
  
    <div class="todo-stats">
      {remaining} item{remaining === 1 ? '' : 's'} remaining
    </div>
    
    <ul class="todo-list" role="list">
      {#each todos as todo (todo.id)}
        <li class:done={todo.done}>
          <label>
            <input 
              type="checkbox" 
              checked={todo.done}
              onclick={() => toggle_todo(todo.id)}
            >
            <span>{todo.text}</span>
          </label>
          <button 
            onclick={() => delete_todo(todo.id)}
            class="delete-btn"
            aria-label="Delete todo"
          >
            ×
          </button>
        </li>
      {/each}
    </ul>
  </main>
  
  <style>
  .todo-app {
    max-width: var(--size-content-3);
    margin: var(--size-7) auto;
    padding: var(--size-3);
  }
  
  h1 {
    margin-bottom: var(--size-5);
    font-size: var(--font-size-5);
  }
  
  .add-form {
    display: flex;
    gap: var(--size-2);
    margin-bottom: var(--size-4);
  }
  
  .add-form input {
    flex: 1;
    padding: var(--size-2);
    border: var(--border-size-1) solid var(--gray-4);
    border-radius: var(--radius-2);
  }
  
  button {
    padding: var(--size-2) var(--size-4);
    background: var(--blue-6);
    color: white;
    border: none;
    border-radius: var(--radius-2);
    cursor: pointer;
  }
  
  button:hover {
    background: var(--blue-7);
  }
  
  .todo-stats {
    color: var(--gray-6);
    margin-bottom: var(--size-3);
  }
  
  .todo-list {
    list-style: none;
    padding: 0;
  }
  
  .todo-list li {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: var(--size-2);
    border-bottom: var(--border-size-1) solid var(--gray-3);
  }
  
  .todo-list li label {
    display: flex;
    align-items: center;
    gap: var(--size-2);
    flex: 1;
  }
  
  .todo-list li.done span {
    text-decoration: line-through;
    color: var(--gray-5);
  }
  
  .delete-btn {
    background: none;
    border: none;
    color: var(--red-6);
    font-size: var(--font-size-4);
    padding: var(--size-1);
  }
  
  .delete-btn:hover {
    color: var(--red-7);
  }
  </style>