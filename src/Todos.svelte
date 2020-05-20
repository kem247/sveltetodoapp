<script>
  import TodoItem from "./TodoItem.svelte";


  let newTodoTitle = '';
  let currentFilter = 'all';
  let nextId = 4;

  let todos = [
    {
      id: 1,
      title: "First on my list...",
      completed: false
    },
    {
      id: 2,
      title: "second...",
      completed: false
    },
    {
      id: 3,
      title: "third...",
      completed: false
    }
  ];

  function addTodo(event) {
    if (event.key === 'Enter') {
      todos = [
        ...todos,
        {
          id: nextId,
          completed: false,
          title: newTodoTitle
        }
      ];

      nextId = nextId + 1;
      newTodoTitle = "";
    }
  }

  $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;
  $: filteredTodos =
    currentFilter === 'all' ? todos : currentFilter === 'completed'
      ? todos.filter(todo => todo.completed)
      : todos.filter(todo => !todo.completed);

  function checkAllTodos(event) {
    todos.forEach(todo => todo.completed = event.target.checked);
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
    const updatedTodo = {...todos[todoIndex], completed: !todos[todoIndex].completed };

    todos = [
      ...todos.slice(0, todoIndex),
      updatedTodo,
      ...todos.slice(todoIndex + 1)
    ];
  }



</script>

<style>
@import 
url(https://fonts.googleapis.com/css?family=Khula:700);

h1 {
   color: #7c795d; 
   font-family: 'Trocchi', serif; 
   font-size: 45px; font-weight: normal; 
   line-height: 48px; 
   margin: 0;
}

body {
  background-color: #fffcde;
}
  .container {
    max-width: 800px;
    margin: 10px auto;
  }

  .todo-input {
    width: 100%;
    padding: 10px, 20px;
    font-size: 18px;
    margin-bottom: 20px;
    border: 0.1875em solid #0F1C3F;
    box-shadow: 0.375em 0.375em 0 0 rgba(15, 28, 63, 0.125);

  }
  .inner-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 15px;
    margin-bottom: 13px;
    font-family: 'Source Sans Pro';
    color: #7c795d;
  }

  .inner-container-input {
    margin-right: 12px;
  }

  button {
    font-size: 14px;
    text-transform: uppercase;
    padding: 1.25em 2em;
    border: 2px solid;
    border-radius: 0.75em;
    transform-style: preserve-3d;
    transition: transform 150ms cubic-bezier(0, 0, 0.58, 1), background 150ms cubic-bezier(0, 0, 0.58, 1);
    font-weight: 600;
    background-color: white;
    appearance: none;
    font-family: 'Source Sans Pro';
    
  }
  
 button::before {
   position: absolute;
   content: '';
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    border-radius: inherit;
    background: #ff9900;
    box-shadow: 0 0 0 2px , 0 0.625em 0 0 #ffcc66 ;
    transform: translate3d(0, 0.75em, -1em);
    transition: transform 150ms cubic-bezier(0, 0, 0.58, 1), box-shadow 150ms cubic-bezier(0, 0, 0.58, 1);
 }

  button:hover {
    background: #ffcc66;
    transform: translate(0, 0.25em);
      
  }

  button:active {
    transform: translate(0em, 0.75em);
  }
  button:focus {
    outline: none;
  }

  .active {
    background:#ffca7b;
  }

  .hidden {
  opacity:0;
}
</style>


<body>
<div class="container">
<h1>Things To Do</h1>
<br>
  <input
    type="text"
    class="todo-input"
    placeholder="Insert todo item..."
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
      <label>
        <input
          class="inner-container-input"
          type="checkbox"
          on:change={checkAllTodos} />
      </label>
      Check All Todos
    </div>
    <div>{todosRemaining} items left</div>
  </div>

  <div class="inner-container">
    <div>
      <button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}">All</button>
      <button on:click={() => updateFilter('active')} class:active="{currentFilter === 'active'}">Active</button>
      <button on:click={() => updateFilter('completed')} class:active="{currentFilter === 'completed'}">Completed</button>
    </div>

    <dir>
      <button on:click={clearCompleted}>Clear Completed</button>
    </dir>

  </div>
</div>
</body>
