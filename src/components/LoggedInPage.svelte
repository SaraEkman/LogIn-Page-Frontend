<script>
    import { fade, slide, scale } from 'svelte/transition';
    import Button from '../styles/Button.svelte';
    let subscribe = false; 
    let showMessageTrue = false
    let showMessageFalse = false

    const handleClickUpdate = async()=>{    
        let _id = localStorage.getItem('_id')
        let updateInfo = {
            _id,
            subscribe
        }
        console.log(updateInfo);

        let respons = await fetch('https://loginpagebackend.herokuapp.com/user/update',{
            headers: {
                'Content-Type': 'application/json'
            },
            method: 'PUT',
            body: JSON.stringify(updateInfo)
        })
        let data = await respons.json()
        console.log(data);
        if (data === 'update subscribe to true'){
            showMessageTrue = true
            showMessageFalse = false
        }else {
            showMessageFalse = true
             showMessageTrue= false
        }
    }

    const handleLoggedOut =()=>{
        localStorage.clear('_id')
        location.reload()
    }

</script>

{#if showMessageTrue}
    <h3>lyckas updatering, tack för din prenumation</h3>
    {:else if showMessageFalse}
    <h3>lyckas updatering, tyvärr du har ingen prenumation</h3>
{/if}
<div in:fade out:scale|local>
    <form on:submit|preventDefault={handleClickUpdate}>
        <label for="checkbox">Prenumerera</label>
        <input id="checkbox" type="checkbox" bind:checked={subscribe}>
        <Button inverse={true} type="secondary" >Updatera</Button>
    </form>
    <Button inverse={true} on:click={handleLoggedOut}>Logga ut</Button>
</div>

<style>
    div {
        margin: 40px auto;
        text-align: center;
    }
    form {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 3em;
        position: relative;
    }
    label {
        align-self: center;
      
    }
input {
    position: absolute;
        top: 21px;
}
    h3 {
        text-align: center;
        color: #4f4f03;
    }
</style>