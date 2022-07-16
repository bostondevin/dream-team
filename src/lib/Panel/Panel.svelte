<script>
	import { onMount } from 'svelte';
	import { nanoid } from 'nanoid';

	import { activeNodeStore, edgesStore, nodesStore } from '$lib/app-store';

	export let currentPage = '';

	let value = '';

	let active = {};

	const submit = () => {
			const newId = nanoid(11);
			nodesStore.update((d) => {
				return [...d, ...[{ id: newId, label: value }]];
			});
			setTimeout(() => {
				edgesStore.update((d) => {
					return [...d, ...[{ source: $activeNodeStore, target: newId }]];
				});
				value = '';
			}, 100);
		},
		change = (key) => {
			/*
			nodesStore.update((d) => {
				return d.map((item) => {
					if (item.id !== active.id) {
						return { ...item, ...active };
					} else {
						return item;
					}
				});
			});
*/
			console.log(key);
			console.log(active[key]);
		};

	activeNodeStore.subscribe((activeNode) => {
		if (activeNode) active = $nodesStore.find((d) => d.id === activeNode);
	});

	onMount(() => {});
</script>

{#if active}
	<h2 class="px-3 mt-3 mb-3 text-3xl">{active.label}</h2>

	<div class="grid grid-cols-2 gap-2 px-3 text-sm">
		{#each Object.entries(active) as [key, value]}
			{#if key !== '__threeObj' && key !== 'index' && key !== 'x' && key !== 'y' && key !== 'z' && key !== 'vx' && key !== 'vy' && key !== 'vz'}
				<div class="grid mb-2">
					<label for="search" class="text-xs block mb-1 capitalize">{key}</label>
					<input
						type="text"
						bind:value={active[key]}
						id="search"
						class="block p-2 w-full rounded bg-slate-600"
						required
						on:keyup={(e) => {
							change(key);
						}}
					/>
				</div>
			{/if}
		{/each}
	</div>

	<form class="p-3" on:submit|preventDefault={(e) => submit()}>
		<label for="search" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
			>{decodeURI(currentPage.replaceAll('/', ' '))}</label
		>
		<div class="relative">
			<!--div class="flex absolute inset-y-0 left-0 items-center pl-3 pointer-events-none">
			<i class="fas fa-search opacity-50" />
		</div-->
			<input
				type="text"
				bind:value
				id="search"
				class="block p-2 w-full rounded bg-slate-600"
				required
			/>
			<button type="button" on:click={(e) => submit()} class="absolute right-2.5 bottom-2.5"
				><i class="fas fa-plus" /></button
			>
		</div>
	</form>
{/if}
