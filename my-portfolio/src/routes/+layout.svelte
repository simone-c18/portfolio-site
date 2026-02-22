<script lang="ts">
	import favicon from '$lib/assets/cat.png';
	import "../app.css";

	let { children } = $props();
</script>

<div class="min-h-screen bg-slate-900 text-white">
  {@render children()}
</div>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<!-- {@render children()} -->
