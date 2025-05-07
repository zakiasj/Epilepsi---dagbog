<script>
  import { onMount } from 'svelte';

  let seizures = [];
  let date = '';
  let time = '';
  let comment = '';

  onMount(() => {
    const stored = localStorage.getItem('seizureLog');
    if (stored) {
      seizures = JSON.parse(stored);
    }
  });

  function addSeizure() {
    if (!date || !time || !comment) return alert("Udfyld dato, tidspunkt og type af anfald.");
    seizures = [...seizures, { date, time, comment }];
    localStorage.setItem('seizureLog', JSON.stringify(seizures));
    date = time = comment = '';
  }

  function clearSeizures() {
    if (confirm("Er du sikker på, at du vil slette alle registreringer?")) {
      seizures = [];
      localStorage.removeItem('seizureLog');
    }
  }
</script>

<style>
  .container {
    max-width: 600px;
    margin: auto;
    padding: 2rem;
    font-family: sans-serif;
  }
  input, textarea, button {
    width: 100%;
    margin: 0.5rem 0;
    padding: 0.75rem;
    font-size: 1rem;
  }
  .entry {
    border-bottom: 1px solid #ccc;
    margin: 1rem 0;
    padding-bottom: 0.5rem;
  }
  .delete-btn {
    background-color: #e53935;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
</style>

<div class="container">
  <h1>Epilepsi Dagbog</h1>
  <p>Registrér dine anfald</p>

  <label>Dato</label>
  <input type="date" bind:value={date} />

  <label>Tidspunkt</label>
  <input type="time" bind:value={time} />

  <label>Hvilken type anfald havde du?</label>
  <textarea rows="3" bind:value={comment} placeholder="Beskriv typen af anfald, f.eks. fokalt, generaliseret, mv."></textarea>

  <button on:click={addSeizure}>Gem anfald</button>

  <h2>Registrerede anfald</h2>
  {#if seizures.length === 0}
    <p>Ingen anfald registreret endnu.</p>
  {:else}
    {#each seizures as s}
      <div class="entry">
        <strong>{s.date} kl. {s.time}</strong><br />
        {s.comment}
      </div>
    {/each}
    <button class="delete-btn" on:click={clearSeizures}>Slet alle registreringer</button>
  {/if}
</div>
