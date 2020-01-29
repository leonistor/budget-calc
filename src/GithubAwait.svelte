<script>
  async function getUsers() {
    let userData = await fetch("https://api.github.com/users");
    let githubUsers = await userData.json();
    return githubUsers;
  }
</script>

{#await getUsers()}
  <!-- promise is pending -->
  <div class="loading loading-lg" />
{:then users}
  <!-- promise was fulfilled -->
  {#each users as { login, avatar_url, url }}
    <div class="chip mr-2">
      <img class="avatar avatar-sm" src={avatar_url} alt={login} />
      <a href={url} class="text-dark" target="_blank">
        {login}
        <i class="icon icon-forward mx-1 text-gray" />
      </a>
    </div>
  {/each}
{/await}
