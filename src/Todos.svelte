<script>
  import TodoItem from "./TodoItem.svelte";

  let newTodoTitle = "";
  let currentFilter = "all";
  let nextId = 4;

  let todos = [
    {
      id: 1,
      title: "Mon premier titre",
      completed: false
    },
    {
      id: 2,
      title: "Mon deuxieme titre",
      completed: false
    },
    {
      id: 3,
      title: "Mon troisieme titre",
      completed: false
    }
  ];

  function addTodo(event) {
    if (event.key === "Enter") {
      todos = [
        ...todos,
        {
          id: nextId,
          title: newTodoTitle,
          completed: false
        }
      ];

      nextId++;
      newTodoTitle = "";
    }
  }

  $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;
  $: filteredTodos =
    currentFilter === "all"
      ? todos
      : currentFilter === "completed"
      ? todos.filter(todo => todo.completed)
      : todos.filter(todo => !todo.completed);

  function checkAllTodos(event) {
    todos.forEach(todo => (todo.completed = event.target.checked));
    todos = todos;
  }

  function updateFilter(newFilter) {
    currentFilter = newFilter;
  }

  function clearCompleted() {
    todos = todos.filter(todo => !todo.completed);
  }

  function handleDeleteTodo(event) {
    todos = todos.filter(todo => todo.id !== event.detail.id);
  }

  function handleToggleComplete(event) {
    const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
    const updateTodo = {
      ...todos[todoIndex],
      completed: !todos[todoIndex].completed
    };

    todos = [
      ...todos.slice(0, todoIndex),
      updateTodo,
      todos.slice(todoIndex + 1)
    ];
  }
</script>

<style>
  .container {
    max-width: 800px;
    margin: 10px auto;
  }
  .todo-input {
    width: 100%;
    padding: 10px 20px;
    font-size: 18px;
    margin-bottom: 20px;
  }

  .inner-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 15px;
    margin-bottom: 13px;
  }

  .inner-container-input {
    margin-right: 12px;
  }

  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
  }

  button:hover {
    background: lightseagreen;
  }

  button:focus {
    outline: none;
  }

  .active {
    background: lightseagreen;
  }
</style>

<div class="container">

  <h2>Svelte Todo App</h2>
  <input
    type="text"
    class="todo-input"
    placeholder="Entrez la tache..."
    bind:value={newTodoTitle}
    on:keydown={addTodo} />

  {#each filteredTodos as todo}
    <div class="todo-item">
      <TodoItem
        {...todo}
        on:deleteTodo={handleDeleteTodo}
        on:toggleComplete={handleToggleComplete} />
    </div>
  {/each}

  <div class="inner-container">
    <div>
      <label for="">
        <input
          type="checkbox"
          name=""
          id=""
          class="inner-container-input"
          on:change={checkAllTodos} />
        Check All Todos
      </label>
    </div>
    <div>{todosRemaining} items left</div>
  </div>

  <div class="inner-container">
    <div>
      <button
        on:click={() => updateFilter('all')}
        class:active="{currentFilter === 'all'}">
        All
      </button>
      <button
        on:click={() => updateFilter('active')}
        class:active="{currentFilter === 'active'}">
        Active
      </button>
      <button
        on:click={() => updateFilter('completed')}
        class:active="{currentFilter === 'completed'}">
        Completed
      </button>
    </div>
    <dir>
      <button on:click={clearCompleted}>Clear Completed</button>
    </dir>
  </div>
</div>
