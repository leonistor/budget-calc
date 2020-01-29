<script>
  import Title from "./Title.svelte";
  import { blur, slide, scale, fade, fly } from "svelte/transition";
  export let active = false;
  export let isEditing;
  export let addExpense;
  export let editExpense;

  export let name = "";
  export let amount = null;

  $: isEmpty = !name || !amount;

  function toggleModal() {
    active = !active;
  }

  function handleSubmit() {
    if (isEditing) {
      editExpense({ name, amount });
    } else {
      addExpense({ name, amount });
    }
    name = "";
    amount = null;
    active = false;
  }
</script>

<!-- <button class="btn btn-primary" on:click={toggleModal}>Add expense</button> -->

<div class="modal modal-sm" class:active id="modal-id" transition:slide>

  <div class="modal-overlay" on:click={toggleModal} transition:blur />

  <div class="modal-container">

    <div class="modal-header">
      <button class="btn btn-link s-circle float-right" on:click={toggleModal}>
        <i class="icon icon-cross" />
      </button>

      <Title title="add expense" />
    </div>

    <form on:submit|preventDefault={handleSubmit} autocomplete="off">

      <div class="modal-body">
        <div class="content">

          <div class="form-group">
            <label for="name" class="form-label">Name</label>
            <input bind:value={name} type="text" id="name" class="form-input" />
          </div>

          <div class="form-group">
            <label for="amount" class="form-label">Amount</label>
            <input
              bind:value={amount}
              type="number"
              id="name"
              class="form-input" />
          </div>

          {#if isEmpty}
            <div class="form-group">
              <p class="text-error text-center">Please fill out all fields.</p>
            </div>
          {/if}

        </div>
      </div>

      <div class="modal-footer">
        <button
          disabled={isEmpty}
          type="submit"
          class="btn btn-primary btn-block">
          {#if isEditing}Edit expense{:else}Add expense{/if}
        </button>
      </div>

    </form>

  </div>
</div>
