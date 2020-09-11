<script>
    import { onMount } from "svelte";
    import User from "./User.svelte";

    let users;

    onMount(async () => {
    await fetch(`https://api.randomuser.me/?results=5`)
      .then(r => r.json())
      .then(data => {
        users = data.results;
      });
  })
</script>

{#if users}
  {#each users as user }
    <ul>
      <li>    
        <User {user} />
      </li>
    </ul>
  {/each}
{:else}
  <p class="loading">loading...</p>
{/if}