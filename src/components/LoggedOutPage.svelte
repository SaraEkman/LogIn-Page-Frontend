<script>
  import { fade, slide, scale } from 'svelte/transition'
  import LogInForm from './LogInForm.svelte'
  import CreateUserForm from './CreateUserForm.svelte'
  import Button from '../styles/Button.svelte'
  let showLogIn = false
  let showNUser = false
  let ErrorNotFoundUser = false
  let ErrorFoundUser = false
  let ableToLogIn = false
  let emptyErr = false
  let ErrorPass = false

  const toggleLogIn = () => {
    showLogIn = !showLogIn
    showNUser = false
    ErrorFoundUser = false
    ableToLogIn = false
    emptyErr = false
    ErrorPass = false
  }
  const toggleCreateNUser = () => {
    showNUser = !showNUser
    showLogIn = false
    ErrorNotFoundUser = false
    emptyErr = false
    ableToLogIn = false
    ErrorPass = false
  }

  const createUser = async (e) => {
    console.log(e.detail)
    let response = await fetch('https://loginpagebackend.herokuapp.com/user/create', {
      headers: {
        'Content-Type': 'application/json',
      },
      method: 'POST',
      body: JSON.stringify(e.detail),
    })
    let data = await response.json()
    console.log(data)

    if (data === 'user in use') {
      ErrorFoundUser = true
      emptyErr = false
      ErrorPass = false
      ableToLogIn = false
    } else if (data === 'new user created') {
      ErrorFoundUser = false
      ableToLogIn = true
      emptyErr = false
      ErrorPass = false
    } else if (data.status === 'empty') {
      emptyErr = true
      ErrorFoundUser = false
      ableToLogIn = false
      ErrorPass = false
    }
  }

  const LogInUser = async (e) => {
    console.log(e.detail)
    let response = await fetch('https://loginpagebackend.herokuapp.com/user/logIn', {
      headers: {
        'Content-Type': 'application/json',
      },
      method: 'POST',
      body: JSON.stringify(e.detail),
    })
    let data = await response.json()
    console.log(data)
    if (data.status == 'empty') {
      emptyErr = true
      ableToLogIn = false
      ErrorNotFoundUser = false
      ErrorPass = false
    } else if (data.status === 'found user') {
      localStorage.setItem('_id', data._id)
      location.reload()
    } else if (data.status == 'Not found user') {
      ErrorNotFoundUser = true
      emptyErr = false
      ableToLogIn = false
      ErrorPass = false
    } else if ((data.status = 'password not found')) {
      ErrorPass = true
      ErrorNotFoundUser = false
      emptyErr = false
      ableToLogIn = false
    }
  }
</script>

<style>
  main {
    max-width: 950px;
    margin: 40px auto;
  }
  div {
    display: flex;
    gap: 1em;
    justify-content: center;
  }
  h3 {
    color: crimson;
    text-align: center;
  }
</style>

<main in:fade out:scale|local>
  <div>
    <Button type="secondary" on:click={toggleLogIn}>Logga in</Button>
    <Button type="secondary" on:click={toggleCreateNUser}>
      Skapa användare
    </Button>
  </div>
  {#if ErrorNotFoundUser}
    <h3>Användaren finns inte försök registrera ett konto!</h3>
  {:else if ErrorFoundUser}
    <h3>Du har redan inlogningsuppgifter, logga in istället!</h3>
  {:else if ableToLogIn}
    <h3>Du kan logga in nu.</h3>
  {:else if emptyErr}
    <h3>Fyll i formuläret tack!</h3>
  {:else if ErrorPass}
    <h3>Har du glömt lösenordet!</h3>
  {/if}
  <LogInForm {showLogIn} on:logInUser={LogInUser} />
  <CreateUserForm {showNUser} on:createUser={createUser} />
</main>
