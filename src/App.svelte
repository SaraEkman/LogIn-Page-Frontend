<script>
  import Header from './components/Header.svelte'
  import Footer from './components/Footer.svelte'
  import LoggedInPage from './components/LoggedInPage.svelte'
  import LoggedOutPage from './components/LoggedOutPage.svelte'
  let getLS = localStorage.getItem('_id')
</script>

<style>

</style>

<Header />
{#if getLS}
  <LoggedInPage />
{:else if getLS === null}
  <LoggedOutPage />
{/if}
<Footer />
