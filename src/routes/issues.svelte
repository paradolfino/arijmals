<script>
  import * as axios from "axios";
  import { onMount } from "svelte";

  onMount(() => {
    axios
      .get("http://localhost:8081/projects")
      .then(function(response) {
        console.log(response);
      })
      .catch(function(error) {
        console.log(error);
      });
  });
</script>

<svelte:head>
  <title>Sapper project template</title>
</svelte:head>
<div>Hello world!</div>
