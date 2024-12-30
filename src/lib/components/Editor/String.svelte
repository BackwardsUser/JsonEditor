<script>
	import { onMount } from "svelte";

  export let key, value;
</script>

<tr>
  <td>{key}</td>
  <td><input class="text-black"  type="text" name={key} id={key} value={value}></td>
</tr>