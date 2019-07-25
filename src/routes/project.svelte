<script>
  import * as axios from "axios";
  import { onMount } from "svelte";
  let path = "http://localhost:8081/search?project=";
  let issues = [];

  onMount(() => {
    let key;
    let urlParams = new URLSearchParams(window.location.search);
    if (urlParams.has("key")) {
      key = urlParams.get("key");
      axios
        .get(path + key + "&maxResults=1000")
        .then(function(response) {
          if (response && response.data) {
            let res = response.data;
            issues = res.issues.filter(item => {
              if (item.fields.subtasks.length > 0) {
                return true;
              }
            });
          }
          console.log(issues);
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  });
</script>

{#each issues as issue}
  <p>{issue.key} : {issue.fields.summary}</p>
{/each}
