<script>
	import { goto } from '$app/navigation';
	import JsonInput from '$lib/components/JsonInput.svelte';
  import { testJSON } from '$lib/helpers/json';
	import { jsonStore } from '$lib/stores/json';
	import { error } from '@sveltejs/kit';

  function submit(ev) {
    const usrJSON = document.getElementById("editor").innerText
    const res = testJSON(usrJSON);
    if (!res)
      error(400);
    jsonStore.set(JSON.parse(usrJSON));
    goto("/viewer");
  }
</script>

<div class="mt-4 w-full flex items-center flex-col">
	<h1 class="text-4xl text-slate-200">Welcome to SvelteKit</h1>
	<JsonInput></JsonInput>
  <button  on:click={submit} class="min-w-min w-80 rounded-lg text-white bg-cyan-700 px-6 py-2 shadow-cyan-950 hover:shadow-2xl hover:bg-cyan-600">Edit JSON</button>
</div>
