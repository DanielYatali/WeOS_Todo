<script>
  import { endpoints } from "$lib/endpoints";
  import { onMount } from "svelte";

  let response = {};
  onMount(() => {
    let blog = {
      //   username: "Daniel",
      userId: 4,
      title: "fits todo",
      description: "a blog from daniel",
      url: "some url",
    };
    (async () => {
      try {
        const rawResponse = await fetch(endpoints.server + "/todos", {
          method: "POST",
          headers: {
            Accept: "application/json",
            // Authorization: 'JWT ' + sender.token,
            "Content-Type": "application/json",
          },
          body: JSON.stringify(blog),
        });
        response = await rawResponse.json();
        console.log(response);
      } catch (error) {
        console.error(error);
      }
    })();
  });
</script>

<div class="p-4 mx-auto text-center">
  {response["message"]}
  <h1
    class="max-w-xs mx-auto my-16 text-6xl font-thin text-red-500 uppercase sm:max-w-none"
  >
    Hello world!
  </h1>

  <p class="max-w-xs mx-auto my-8 sm:max-w-none">
    Visit the <a href="https://svelte.dev">svelte.dev</a> to learn how to build Svelte
    apps.
  </p>
</div>
