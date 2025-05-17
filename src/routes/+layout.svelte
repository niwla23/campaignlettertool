<script lang="ts">
	import { writable } from 'svelte/store';
	import '../app.css';

	let { children } = $props();

	let isDev = import.meta.env.DEV;
</script>

<svelte:head>
	{#if !isDev}
		<script
			defer
			src="https://umami.cloudserver.click/homephoner"
			data-website-id="58cd4073-5547-4afb-a07a-7d79e9fb91cb"
		></script>
	{/if}
</svelte:head>
{@render children()}
