<script>
  import { getContext } from "svelte";

  export let name;
  export let id;
  export let amount;

  let displayAmount = false;

  function toggleDisplayAmount() {
    displayAmount = !displayAmount;
  }

  const { remove } = getContext("expense");
</script>

<article class="single-expense">
  <div class="expense-info">
    <h2>
      {name}
      <button class="amount-btn" on:click={toggleDisplayAmount}>
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount}
      <h4>amount : ${amount}</h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button class="expense-btn edit-btn"> <i class="fas fa-pen" /> </button>
    <button class="expense-btn delete-btn" on:click={() => remove(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>
