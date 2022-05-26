<script>
  export let type = 'primary';
  export let inverse = false;
</script>

<button  class={type} class:inverse={inverse} on:click>
  <slot></slot>
</button>

<style>
  button {
    border: 0;
    cursor: pointer;
    border-radius: 30px 60px;
    padding: 8px 12px;
    font-weight: bold;
    box-shadow: rgba(0, 0, 0, 0.09) 0px 2px 1px, rgba(0, 0, 0, 0.09) 0px 4px 2px, rgba(0, 0, 0, 0.09) 0px 8px 4px, rgba(0, 0, 0, 0.09) 0px 16px 8px, rgba(0, 0, 0, 0.09) 0px 32px 16px;
    transition: transform 0.5s ease-in-out;
  }

  button:hover {
    color: #fff;
    transform: scale(1.1);
    transition: transform 0.5s ease-in-out;
  } 

  .primary {
    background: #ff3e00;
    color:black
  }

  .secondary {
    background: #45c496;
    color:black
  }

  .primary.inverse {
    color: #ff3e00;
    background: #fff;
    border: 2px solid #ff3e00;
    margin-top: 1em;
  }

  .secondary.inverse {
    color: #45c496;
    background-color: #fff;
    border: 2px solid #45c496;
  }
</style>