<!-- Homepage of todo app -->
<script>
  import { endpoints } from "$lib/endpoints";
  import { onMount } from "svelte";
  import TodoInput from "../components/todoInput.svelte";

  let response = {};
  let todos = [];
  let description = "";
  let title = "";

  //When component mounted fetch all todos
  onMount(() => {
    getTodos();
  });

  //Fetches all todos from WeOS server
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
      // console.log(todos);
    } catch (error) {
      console.error(error);
    }
  };

  //Creates a todo
  const createTodo = async () => {
    //Prevent user from making blank todo
    if (title != "" && description != "") {
      let todo = {
        title: title,
        description: description,
        userId: 0,
      };
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
        // console.log(response);
        title = "";
        description = "";
        //Fetches updated todo list
        getTodos();
      } catch (error) {
        console.error(error);
      }
    }
  };

  //Removes a todo
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
      // console.log(response);
      //Fetches updates todo list
      getTodos();
    } catch (error) {
      console.error(error);
    }
  };

  //Edits todo by sending updated values to WeOS server
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
      // console.log(response);
      //Fetching updated todo list
      getTodos();
    } catch (error) {
      console.error(error);
    }
  };
</script>

<div class="h-100 w-full flex items-center justify-center bg-teal-lightest">
  <div class="bg-red rounded shadow p-6 m-4 w-full lg:w-3/4 bg-gray-200">
    <div class="mb-4">
      <h1 class="text-3xl text-center text-gray-700 pb-4">Todo List</h1>
      <div class="w-full flex flex-col gap-2 sm:flex-row items-center">
        <!-- binds the title variable to the users input -->
        <input
          bind:value={title}
          class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-gray-600"
          placeholder="Title"
        />
        <!-- binds the description variable to the users input -->
        <input
          bind:value={description}
          class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-gray-600"
          placeholder="Description"
        />
        <button
          on:click|preventDefault={createTodo}
          class="flex-no-shrink p-2 rounded-lg text-white bg-gray-500 hover:text-red hover:bg-gray-700"
          >Add</button
        >
      </div>
      <br />
      <div class="flex flex-col-reverse">
        {#each todos as todo, Index (todo.id)}
          <div class="shadow m-2 bg-gray-100 rounded-lg">
            <TodoInput {todo} {removeTodo} {editTodo} />
          </div>
        {/each}
      </div>
    </div>
  </div>
</div>

<svelte:head>
  <style>
    body {
      --tw-bg-opacity: 1;
      background-color: rgb(243 244 246 / var(--tw-bg-opacity));
    }
  </style>
</svelte:head>
