<script context="module">
	export const prerender = true;
</script>

<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';

	import { activeNodeStore, edgesStore, nodesStore } from '$lib/app-store';

	$: currentPage = $page.url.pathname ? $page.url.pathname.toLowerCase() : null;

	import MindMap from '$lib/MindMap/MindMap.svelte';
	import Panel from '$lib/Panel/Panel.svelte';
	import Tabs from '$lib/Tabs.svelte';

	onMount(() => {
		const pathParts = currentPage.split('/');

		pathParts.forEach((data, i) => {
			if (data) {
				nodesStore.update((d) => {
					return [...d, ...[{ id: pathParts[i], label: decodeURI(pathParts[i]) }]];
				});

				if (i > 1) {
					edgesStore.update((d) => {
						return [...d, ...[{ source: pathParts[i - 1], target: pathParts[i] }]];
					});
				}
			}
		});

		activeNodeStore.set(pathParts[1]);
	});
</script>

<svelte:head>
	<title>Dream for Fi</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<div class="flex grow text-white">
	<div class="flex-none w-2/3 h-full bg-slate-600">
		<MindMap {currentPage} />
	</div>
	<div class="flex-initial w-1/3 h-full bg-slate-800 z-10">
		<Panel {currentPage} />
	</div>
</div>

<Tabs {currentPage} />
