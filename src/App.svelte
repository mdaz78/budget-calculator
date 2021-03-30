<script>
  import { setContext } from "svelte";

  import Navbar from "./Components/Navbar.svelte";
  import ExpenseList from "./Components/ExpenseList.svelte";

  import expensesData from "./expenses.js";

  let expenses = expensesData;

  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id);
  }

  setContext("remove", removeExpense);
</script>

<Navbar />

<main class="content">
  <ExpenseList {expenses} />
</main>
