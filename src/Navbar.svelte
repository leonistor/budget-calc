<script>
  export let formActive;
</script>

<header class="navbar bg-gray p-2">
  <section class="navbar-section">
    <a href="/" class="navbar-brand mr-2 text-uppercase text-bold">
      Budget calculator
    </a>
  </section>

  <section class="navbar-section">
    <button class="btn btn-primary" on:click={() => (formActive = true)}>
      <i class="icon icon-plus mr-1" />
      Add expense
    </button>
  </section>
</header>
