<script>
  import { getContext } from "svelte";
  import Title from "./Title.svelte";
  let name = "";
  let amount = null;
  $: isEmpty = !name || !amount;

  const expense = getContext("expense");

  function handleSubmit() {
    expense.add({ name, amount });
    name = "";
    amount = null;
  }
</script>

<section class="form">
  <Title title="Add Expense" />
  <form on:submit|preventDefault={handleSubmit} class="expense-form">
    <div class="form-control">
      <label for="name">Name</label>
      <input type="text" id="name" bind:value={name} />
    </div>

    <div class="form-control">
      <label for="amount">Amount</label>
      <input type="number" id="amount" min="0" bind:value={amount} />
    </div>

    {#if isEmpty}
      <p class="form-empty">Please fill out all form fields</p>
    {/if}

    <button
      type="submit"
      class="btn btn-block"
      disabled={isEmpty}
      class:disabled={isEmpty}>Add Expense</button>
    <button type="button" class="close-btn">
      <i class="fas fa-times" />
      Close
    </button>
  </form>
</section>
