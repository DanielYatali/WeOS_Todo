<script>
  import { endpoints } from "$lib/endpoints";
  import { onMount } from "svelte";
  import TodoInput from "../components/todoInput.svelte";
  import todoInput from "../components/todoInput.svelte";

  let response = {};
  let todos = [];
  let description = "";
  let title = "";
  let updateDescription = "";
  let updateTitle = "";
  let editButtonClick = false;
  onMount(() => {
    getTodos();
  });
  const getTodos = async () => {
    try {
      const rawResponse = await fetch(endpoints.server + "/todos", {
        method: "GET",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
      });
      response = await rawResponse.json();
      todos = response.items;
      console.log(todos);
    } catch (error) {
      console.error(error);
    }
  };
  const createTodo = async () => {
    let todo = {
      title: title,
      description: description,
      userId: 0,
    };
    console.log(title);
    try {
      const rawResponse = await fetch(endpoints.server + "/todos", {
        method: "POST",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        body: JSON.stringify(todo),
      });
      response = await rawResponse.json();
      console.log(response);
      getTodos();
    } catch (error) {
      console.error(error);
    }
  };
  const removeTodo = async (id) => {
    try {
      const rawResponse = await fetch(endpoints.server + "/todos/" + id, {
        method: "DELETE",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
      });
      response = await rawResponse.json();
      console.log(response);
      getTodos();
    } catch (error) {
      console.error(error);
    }
  };

  const editTodo = async (updateTodo) => {
    try {
      const rawResponse = await fetch(
        endpoints.server + "/todos/" + updateTodo.id,
        {
          method: "PUT",
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json",
          },
          body: JSON.stringify(updateTodo),
        }
      );
      let response = await rawResponse.json();
      console.log(response);
      getTodos();
    } catch (error) {
      console.error(error);
    }
  };
</script>

<div
  class="h-100 w-full flex items-center justify-center bg-teal-lightest font-sans"
>
  <div class="bg-red rounded shadow p-6 m-4 w-full lg:w-3/4">
    <div class="mb-4">
      <h1 class="text-grey-darkest">Todo List</h1>
      <div class="flex mt-4">
        <input
          bind:value={title}
          class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
          placeholder="Title"
        />
        <input
          bind:value={description}
          class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
          placeholder="Description"
        />
        <button
          on:click|preventDefault={createTodo}
          class="flex-no-shrink p-2 border-2 rounded text-teal border-teal hover:text-red hover:bg-teal"
          >Add</button
        >
      </div>
      <br />
      {#each todos as todo, Index (todo.id)}
        <div>
          <TodoInput {todo} {removeTodo} {editTodo} />
          <!-- <div class="flex mb-4 items-center">
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
          </div> -->
        </div>
        <!-- {#if editButtonClick}
          <div class="flex mt-4">
            <input
              bind:value={title}
              class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
              placeholder="New Title"
            />
            <input
              bind:value={description}
              class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
              placeholder="New Description"
            />
            <button
              on:click|preventDefault={() => editTodo(todo.id)}
              class="flex-no-shrink p-2 border-2 rounded text-teal border-teal hover:text-white hover:bg-teal"
              >Update</button
            >
          </div>
        {/if} -->
      {/each}
    </div>
  </div>
</div>
