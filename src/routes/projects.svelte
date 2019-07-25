<script>
  import * as axios from "axios";
  import { onMount } from "svelte";

  let projects = [];
  let path = "http://localhost:8081/projects";

  onMount(() => {
    axios
      .get(path)
      .then(function(response) {
        if (response && response.data) {
          let res = response.data;
          projects = res.projects;
          console.log(projects);
        }
      })
      .catch(function(error) {
        console.log(error);
      });
  });
</script>

<svelte:head>
  <title>Projects</title>
</svelte:head>

<div class="projectsList">
  {#each projects as project}
    <div class="projectList--Project">
      <div class="projectList--ProjectAvatar">
        <img src={project.avatarUrls['48x48']} alt={project.name} />
      </div>
      <div class="projectList--ProjectMeta">{project.name}</div>
      <a href={'/project?key=' + project.key}>test</a>
    </div>
  {/each}
</div>
