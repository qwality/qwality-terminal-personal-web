<script>
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	// Set Skeleton theme via data-theme attribute

	let { children } = $props();
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>

<div class="bg-surface-950 text-surface-50 min-h-screen font-mono" data-theme="cerberus">
	{@render children()}
</div>
