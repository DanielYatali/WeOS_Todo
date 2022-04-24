<script>
  let updateDescription = "";
  let updateTitle = "";
  export let todo;
  export let editTodo;
  export let setEditButtonClick;
  const editCurrentTodo = () => {
    todo.description = updateDescription;
    todo.title = updateTitle;
    setEditButtonClick();
    editTodo(todo);
  };
</script>

<div>
  <div class="flex m-4">
    <input
      bind:value={updateTitle}
      class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
      placeholder="New Title"
    />
    <input
      bind:value={updateDescription}
      class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
      placeholder="New Description"
    />
    <button
      on:click|preventDefault={editCurrentTodo}
      class="flex-no-shrink p-2 border-2 rounded text-teal border-teal hover:text-white hover:bg-teal"
      >Update</button
    >
  </div>
</div>
