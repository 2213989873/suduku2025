<script>
	import Switch from '../../Utils/Switch.svelte';
	import { activeStrategies as store } from '@sudoku/stores/activeStrategies';
	import { STRATEGIES } from '@sudoku/constants';

	export let hideModal;

	let localStrategies = { ...$store };
    
    // Ensure all current strategies are present in local object
    Object.keys(STRATEGIES).forEach(key => {
        if (localStrategies[key] === undefined) {
            localStrategies[key] = true;
        }
    });

	function handleSave() {
		store.set(localStrategies);
		hideModal();
	}
    
    function toggleAll(value) {
        Object.keys(localStrategies).forEach(key => {
            localStrategies[key] = value;
        });
    }
</script>

<h1 class="text-3xl font-semibold mb-6 leading-none">Select Strategies</h1>

<div class="mb-4 flex space-x-4 text-sm">
    <button class="text-blue-600 hover:text-blue-800 font-semibold" on:click={() => toggleAll(true)}>Select All</button>
    <button class="text-blue-600 hover:text-blue-800 font-semibold" on:click={() => toggleAll(false)}>Deselect All</button>
</div>

<div class="flex flex-col mb-6 space-y-3" style="max-height: 50vh; overflow-y: auto; padding-right: 10px;">
    {#each Object.entries(STRATEGIES) as [key, label]}
	    <Switch bind:checked={localStrategies[key]} text={label} id="strategy-{key}" />
    {/each}
</div>

<div class="flex space-x-3">
	<button class="btn btn-primary flex-grow text-center bg-blue-600 text-white font-bold py-2 px-4 rounded" on:click={handleSave}>Save</button>
	<button class="btn btn-secondary flex-grow text-center bg-gray-300 text-gray-800 font-bold py-2 px-4 rounded" on:click={hideModal}>Cancel</button>
</div>
