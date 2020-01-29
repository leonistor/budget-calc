<script>
  import { blur, slide, scale, fade, fly } from "svelte/transition";
  import { flip } from "svelte/animate";
  import SectionTitle from "./Title.svelte";
  import Expense from "./Expense.svelte";
  export let expenses = [];
</script>

<SectionTitle title="expense list" />
{#each expenses as expense, index (expense.id)}
  <div
    in:fly={{ x: 200, delay: index * 700 }}
    out:fly={{ x: -200 }}
    animate:flip>
    <Expense {...expense} />
  </div>
{:else}
  <h2>No expenses.</h2>
{/each}
