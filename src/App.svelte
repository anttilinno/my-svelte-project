<script>
  import svelteLogo from "./assets/svelte.svg";

  let json;
  let reachValue = false;

  import { onMount } from "svelte";

  let minValue = null;

  function calculateMin() {
    let selectedRows = json.filter((row) => row.selected && row.kogus);
    let coefficient = 1.1 - selectedRows.length * 0.1;
    if (coefficient < 0.5 || reachValue) {
      coefficient = 0.5;
    }

    let min = selectedRows.length
      ? Math.min(...selectedRows.map((row) => row.kogus))
      : null;

    alert(`Minimal value ${min} and coefficient ${coefficient}.`);

    minValue = (min * coefficient).toFixed(2);
  }

  function validateInput(inputValue) {
    return !isNaN(inputValue);
  }

  onMount(async () => {
    const response = await fetch("https://api.npoint.io/5db9004af7a85fa2cbfa");
    json = await response.json();

    json = json.map((row) => {
      row.selected = false;
      row.inputValue = "";
      return row;
    });
  });
</script>

<main>
  <div>
    <a href="https://vitejs.dev" target="_blank" rel="noreferrer">
      <img src="/vite.svg" class="logo" alt="Vite Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank" rel="noreferrer">
      <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
    </a>
  </div>
  <h1>Vite + Svelte</h1>

  <div class="card">
    <div><button on:click={calculateMin}>Calculate Minimum</button></div>
    <div>
      <input type="checkbox" name="reach" bind:checked={reachValue} />Reach XIV
    </div>
    {#if minValue !== null}
      <p>Minimum: {minValue}</p>
    {/if}

    {#if json}
      <table>
        <thead>
          <tr>
            <th>&nbsp;</th>
            <th>Klass</th>
            <th>Kood</th>
            <th>Kogus (kg)</th>
          </tr>
        </thead>
        <tbody>
          {#each json as row}
            <tr>
              <td>
                <input type="checkbox" bind:checked={row.selected} />
              </td>
              <td>{row.klass ? row.klass : ""}</td>
              <td>{row.kood}</td>
              <td>{row.kogus}</td>
            </tr>
          {/each}
        </tbody>
      </table>
    {:else}
      <p>Loading data ...</p>
    {/if}
  </div>

  <p>
    Check out <a
      href="https://github.com/sveltejs/kit#readme"
      target="_blank"
      rel="noreferrer">SvelteKit</a
    >, the official Svelte app framework powered by Vite!
  </p>

  <p class="read-the-docs">Click on the Vite and Svelte logos to learn more</p>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>
