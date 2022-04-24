<script>
  import TodoUpdate from "./todoUpdate.svelte";
  let editButtonClick = false;
  export let todo;
  export let removeTodo;
  export let editTodo;

  const setEditButtonClick = () => {
    editButtonClick = false;
  };
</script>

<div>
  <div class="flex mb-4 items-center">
    <div class="w-full">
      <p class="w-full text-grey-darkest">
        {todo.title}
      </p>
      <p class="w-full text-grey-darkest">
        {todo.description}
      </p>
    </div>
    <button
      on:click|preventDefault={() =>
        (editButtonClick = editButtonClick ? false : true)}
      class="flex-no-shrink p-2 ml-2 border-2 rounded text-red border-red hover:text-white hover:bg-red"
      >Edit</button
    >
    <button
      on:click|preventDefault={() => removeTodo(todo.id)}
      class="flex-no-shrink p-2 ml-2 border-2 rounded text-red border-red hover:text-white hover:bg-red"
      >Remove</button
    >
  </div>
  {#if editButtonClick}
    <TodoUpdate {todo} {editTodo} {setEditButtonClick} />
  {/if}
</div>
