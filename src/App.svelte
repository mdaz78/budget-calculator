<script>
  import { afterUpdate, onMount, setContext } from 'svelte';
  import Navbar from './Components/Navbar.svelte';
  import ExpenseList from './Components/ExpenseList.svelte';
  import Totals from './Components/Totals.svelte';
  import ExpenseForm from './Components/ExpenseForm.svelte';
  import Modal from './Components/Modal.svelte';

  // import expensesData from './expenses.js';

  let expenses = [];

  let setName = '';
  let setAmount = null;
  let setId = null;

  // toggle form variables
  let isFormOpen = false;

  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((sum, item) => (sum += item.amount), 0);

  // functions
  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id);
  }

  function removeAllExpenses() {
    expenses = [];
  }

  function addExpense({ name, amount }) {
    const expense = {
      id: Math.random() * Date.now(),
      name,
      amount,
    };

    expenses = [expense, ...expenses];
  }

  function setModifiedExpense(id) {
    let expense = expenses.find((expense) => expense.id === id);

    setId = expense.id;
    setAmount = expense.amount;
    setName = expense.name;
    showForm();
  }

  function updateExpense({ name, amount }) {
    expenses = expenses.map((expense) => {
      if (expense.id === setId) {
        return {
          id: setId,
          name,
          amount,
        };
      } else {
        return { ...expense };
      }
    });

    setName = '';
    setAmount = null;
    setId = null;
  }

  function showForm() {
    isFormOpen = true;
  }

  function hideForm() {
    isFormOpen = false;
    setName = '';
    setAmount = null;
    setId = null;
  }

  setContext('expense', {
    remove: removeExpense,
    add: addExpense,
    modify: setModifiedExpense,
    update: updateExpense,
  });

  // local Storage
  function setLocalStorage() {
    const stringifiedExpenses = JSON.stringify(expenses);
    localStorage.setItem('expenses', stringifiedExpenses);
  }

  function getExpensesFromLocalstorage() {
    const stringifiedExpenses = localStorage.getItem('expenses');
    return JSON.parse(stringifiedExpenses);
  }

  onMount(() => {
    expenses = getExpensesFromLocalstorage();
  });

  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<Navbar {showForm} />

<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm name={setName} amount={setAmount} {isEditing} {hideForm} />
    </Modal>
  {/if}
  <Totals title="Total Expenses" {total} />
  <ExpenseList {expenses} />
  {#if expenses.length > 0}
    <button class="btn btn-primary btn-block" on:click={removeAllExpenses}
      >Clear Expenses</button
    >
  {/if}
</main>
