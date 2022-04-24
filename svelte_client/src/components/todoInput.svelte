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

<div class="p-4 pl-6">
  <div class="flex flex-col sm:flex-row gap-2 items-center">
    <div class="w-full">
      <p class="w-full text-lg text-gray-600">
        {todo.title}
      </p>
      <p class="w-full text-gray-500 pl-4">
        {todo.description}
      </p>
    </div>
    <div class="sm:flex gap-3">
      <button
        on:click|preventDefault={() =>
          (editButtonClick = editButtonClick ? false : true)}
        class="flex-no-shrink p-2 rounded-lg text-white bg-gray-500 hover:text-red hover:bg-gray-700"
        >Edit</button
      >
      <button
        on:click|preventDefault={() => removeTodo(todo.id)}
        class="flex-no-shrink p-2 rounded-lg text-white bg-gray-500 hover:text-red hover:bg-gray-700"
        >Remove</button
      >
    </div>
  </div>
  {#if editButtonClick}
    <div>
      <TodoUpdate {todo} {editTodo} {setEditButtonClick} />
    </div>
  {/if}
</div>
