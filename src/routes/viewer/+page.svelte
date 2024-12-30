<script>
	import Boolean from '$lib/components/Editor/Boolean.svelte';
	import String from '$lib/components/Editor/String.svelte';
	import { jsonStore } from '$lib/stores/json.js';
	console.log($jsonStore);
</script>

<h1>Edit away...</h1>

<table class="pure-table pure-table-bordered">
	<thead>
		<tr>
			<th>Key</th>
			<th>Value</th>
		</tr>
	</thead>
	<tbody class="text-white">
		{#each Object.entries($jsonStore) as pair}
			{#if typeof pair[1] === 'boolean'}
				<Boolean key={pair[0]} value={pair[1]}></Boolean>
			{:else if typeof pair[1] === 'string'}
				<String key={pair[0]} value={pair[1]}></String>
        {:else if Array.isArray(pair[1])}
			{/if}
		{/each}
	</tbody>
</table>
