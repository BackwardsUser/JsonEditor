<script>
	import { onMount } from "svelte";

  export let key, value;
</script>

<tr>
  <td>{key}</td>
  <td><input type="checkbox" name={key} id={key} checked={value}></td>
</tr>