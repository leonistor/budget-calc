<script>
  import { onMount } from "svelte";

  let users = [];
  let loading = true;

  onMount(async () => {
    let userData = await fetch("https://api.github.com/users");
    let githubUsers = await userData.json();
    users = githubUsers;
    loading = false;
  });
</script>

{#if loading}
  <div class="loading loading-lg" />
{:else}
  {#each users as { login, avatar_url, url }}
    <div class="chip">
      <img class="avatar avatar-sm" src={avatar_url} alt={login} />
      <a href={url} class="text-dark" target="_blank">
        {login}
        <i class="icon icon-forward mx-1 text-gray" />
      </a>
    </div>
  {/each}
{/if}
