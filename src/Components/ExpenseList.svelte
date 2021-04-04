<script>
  import Title from "./Title.svelte";
  import Expense from "./Expense.svelte";

  export let expenses = [];
</script>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>

<section>
  <Title title="Expense List" />
  <ul>
    {#each expenses as expense}
      <Expense {...expense} />
    {:else}
      <h2>No expenses added to the list</h2>
    {/each}
  </ul>
</section>
