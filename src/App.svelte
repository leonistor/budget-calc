<script>
  import { setContext, onMount, afterUpdate } from "svelte";

  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  // import Github from "./GithubAwait.svelte";

  import shortid from "shortid";

  // data
  // import expensesData from "./expenses.js";

  // variables
  // let expenses = [...expensesData];
  let expenses = [];

  let setName = "";
  let setAmount = null;
  let setId = null;
  let formActive = false;

  // reactive
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, cur) => acc + cur.amount, 0);

  // functions
  function removeExpense(id) {
    expenses = expenses.filter(item => item.id != id);
    formActive = false;
  }
  function clearExpenses() {
    expenses = [];
  }
  function addExpense({ name, amount }) {
    let expense = { id: shortid.generate(), name, amount };
    expenses = [expense, ...expenses];
    formActive = false;
  }
  function setModifiedExpense(id) {
    let expense = expenses.find(item => item.id === id);
    if (expense) {
      setId = expense.id;
      setName = expense.name;
      setAmount = expense.amount;
      formActive = true;
    }
  }
  function editExpense({ name, amount }) {
    expenses = expenses.map(item => {
      return item.id === setId ? { ...item, name, amount } : item;
    });
    setId = null;
    setName = "";
    setAmount = null;
    formActive = false;
  }

  // context
  setContext("remove", removeExpense);
  setContext("modify", setModifiedExpense);

  // local storage
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }

  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });

  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<style>
  #container {
    margin-top: 1rem;
  }
</style>

<main class="container grid-sm">
  <Navbar bind:formActive />
  <div id="container" class="container">
    <Totals title="Total expenses" {total} />
    <div class="divider" />
    <ExpenseForm
      {addExpense}
      {editExpense}
      name={setName}
      amount={setAmount}
      bind:active={formActive}
      {isEditing} />
    <ExpensesList {expenses} />
    <div class="divider" />
    <section class="p-2">
      <button class="btn float-right" on:click={clearExpenses}>
        Clear expenses
      </button>
      <p>&nbsp;</p>
    </section>

    <div class="divider" />
    <!-- <Github /> -->

  </div>
</main>
