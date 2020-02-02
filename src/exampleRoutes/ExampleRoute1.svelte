<script>
  import { fly } from "svelte/transition";
  export let name;
</script>

<div in:fly={{ x: 500, duration: 500 }} out:fly={{ x: -500, duration: 500 }} style='display:inline-block;'>
  <h1>Example Route 1</h1>

  <h4>Hello, {name}</h4>
</div>
