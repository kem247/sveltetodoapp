<script>
  import { createEventDispatcher } from "svelte";
  import { fly } from "svelte/transition";

  export let id;
  export let title;
  export let completed;

  const dispatch =  createEventDispatcher();

  function deleteTodo() {
    dispatch("deleteTodo", {
      id: id
    });
  }

  function toggleComplete() {
    dispatch("toggleComplete", {
      id: id
    });
  }
</script>

<style>
  .todo-item {
    margin-bottom: 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
    color:#ff9900;
    font-family: 'Source Sans Pro';
  }

  .todo-item:hover { color: #ffcc66 }
  .remove-item {
    cursor: pointer;
    margin-left: 15px;
  }
  .remove-item:hover {
    color: lightseagreen;
  }
  .todo-item-left {
    display: flex;
    align-items: center;
  }
  .todo-item-label {
    border: 1px solid white;
    margin-left: 12px;
  }
  .completed {
    text-decoration: line-through;
    color: grey;
  }
</style>

<div class="todo-item">
  <div class="todo-item-left" transition:fly={{ y: 20, duration: 300 }}>
    <input type="checkbox" bind:checked={completed} on:change={toggleComplete} />

    <div class="todo-item-label" class:completed={completed}>{title}</div>
    <!-- prettier-ignore -->
  </div>
  <div class="remove-item" on:click={deleteTodo}>x</div>
</div>
