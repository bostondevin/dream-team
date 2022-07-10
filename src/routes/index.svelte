<script context="module">
	export const prerender = true;
</script>

<script>
	import MindMap from '$lib/MindMap/MindMap.svelte';
	import Panel from '$lib/Panel/Panel.svelte';
</script>

<svelte:head>
	<title>Dream for Fi</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<div class="flex h-full text-white">
	<div class="flex-none w-2/3 h-full bg-slate-500">
		<MindMap />
	</div>
	<div class="flex-initial w-1/3 h-full bg-slate-800">
		<Panel />
	</div>
</div>
