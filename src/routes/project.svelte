<script>
  import * as axios from "axios";
  import { onMount } from "svelte";
  let path = "http://localhost:8081/search?project=";
  let issues2 = "http://localhost:8081/issues?sprintID=";
  let boards = "http://localhost:8081/boards";
  let sprints = "http://localhost:8081/sprints?rapidboard=";
  let issues = [];

  function getIssues(id) {
    axios
      .get(issues2 + id)
      .then(function(response) {
        if (response && response.data) {
          let res = response.data;
          debugger;
        }
        console.log(issues);
      })
      .catch(function(error) {
        console.log(error);
      });
  }

  function getRapidViews(key) {
    axios
      .get(boards)
      .then(function(response) {
        if (response && response.data) {
          let res = response.data;
          if (res.values) {
            let boards = res.values.filter(
              item => item.location.projectKey == key
            );
            if (boards.length > 0) {
              for (let i = 0; i < boards.length; i++) {
                getSprints(boards[i].id);
              }
            }
          }
        }
        console.log(issues);
      })
      .catch(function(error) {
        console.log(error);
      });
  }

  function getSprints(id) {
    axios
      .get(sprints + id)
      .then(function(response) {
        if (response && response.data) {
          let res = response.data;
          if (res.values) {
            let sprints = res.values;
            if (sprints.length > 0) {
              for (let i = 0; i < sprints.length; i++) {
                getIssues(sprints[i].id);
              }
            }
          }
        }
        console.log(issues);
      })
      .catch(function(error) {
        console.log(error);
      });
  }

  onMount(() => {
    let key;
    let urlParams = new URLSearchParams(window.location.search);
    if (urlParams.has("key")) {
      key = urlParams.get("key");
      // getSprints(29);
      getRapidViews(key);
    }
  });
</script>

{#each issues as issue}
  <p>{issue.key} : {issue.fields.summary}</p>
{/each}
