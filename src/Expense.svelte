<script>
  import { getContext } from "svelte";
  import { blur, slide, scale, fade, fly } from "svelte/transition";
  export let name = "",
    amount = 0,
    id = "";
  let open = false;

  function toggleOpen() {
    open = !open;
  }

  const removeExpense = getContext("remove");
  const setModifiedExpense = getContext("modify");
</script>

<style>

</style>

<details {id} class="accordion" bind:open>
  <summary class="accordion-header c-hand">
    <h4>
      {name}
      <i class="icon icon-arrow-right mr1" on:click={toggleOpen} />
    </h4>
  </summary>
  {#if open}
    <div class="accordion-body" transition:slide>
      <div class="tile">
        <div class="tile-content px-2">
          <span class="h5">
            Amount:
            <kbd>{amount}</kbd>
          </span>
        </div>
        <div class="tile-action" on:click={() => setModifiedExpense(id)}>
          <button class="btn btn-primary btn-sm">
            <i class="icon icon-edit" />
          </button>
          <button
            class="btn btn-error btn-sm"
            on:click={() => {
              open = false;
              removeExpense(id);
            }}>
            <i class="icon icon-delete" />
          </button>
        </div>
      </div>
    </div>
  {/if}
</details>
