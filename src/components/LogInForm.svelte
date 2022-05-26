<script>
   import { fade, slide, scale } from 'svelte/transition';
  //  import { flip } from 'svelte/animate';
    import {createEventDispatcher} from 'svelte'
import Button from '../styles/Button.svelte';
    let dispatch = createEventDispatcher()
    let email
    let passWord
    let subscribe = false
  export let showLogIn = false

  const handleClick = () => {
    let user = {
      email,
      passWord,
      subscribe,
    }
    console.log(user)
    dispatch('logInUser', user)

  }
</script>

{#if showLogIn}
<div in:fade out:scale|local>
  <form on:submit|preventDefault={handleClick}>
    <label for="email">Email</label>
    <input id="email" type="email" bind:value={email} />
    <label for="password">Lösenord</label>
    <input id="password" type="password" bind:value={passWord} />
    <label for="newsLetter">Nyhetsbrev</label>
    <input type="checkbox" bind:checked={subscribe} />
    <Button>Logga In</Button>
  </form>
</div>
{/if}

<style>
    div {
       max-width: 950px;
       text-align: center;
    }
    form {
        display: flex;
        align-items: center;
        flex-direction: column;
        gap: 1em;
        margin-top: 1.5em;
    }
</style>
