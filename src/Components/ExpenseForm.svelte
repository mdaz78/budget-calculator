<script>
  import { getContext } from 'svelte';
  import Title from './Title.svelte';

  const expense = getContext('expense');

  export let name = '';
  export let amount = null;
  export let isEditing = false;
  export let hideForm;

  $: isEmpty = !name || !amount;
  $: title = isEditing ? 'Edit Expense' : 'Add Expense';

  function handleSubmit() {
    if (isEditing) {
      expense.update({ name, amount });
    } else {
      expense.add({ name, amount });
    }

    name = '';
    amount = null;
    hideForm();
  }
</script>

<section class="form">
  <Title {title} />
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
      class:disabled={isEmpty}>{title}</button
    >
    <button type="button" class="close-btn" on:click={hideForm}>
      <i class="fas fa-times" />
      Close
    </button>
  </form>
</section>
