<script>
	import Item from '@/lib/Item.svelte';

	export let events;
	export let handleDeleteEvent;

	export let numOfDoneEventsBasedByTimeButInTodoListLib;
</script>

<div>
	{#each events as event, i}
		<div style:background={event.expired && 'rgba(255, 0, 0, 0.4)'}>
			{#if i < numOfDoneEventsBasedByTimeButInTodoListLib}
				<h2 style="text-decoration:line-through;">{event.name}</h2>
			{:else}
				<h2>{event.name}</h2>
			{/if}
			<p>{event.message}</p>
			<h4>{new Date(event.reminderTimestamp).toLocaleString()}</h4>
			{#if event.expired}
				<h5>Event expired</h5>
			{/if}
			<button on:click={() => handleDeleteEvent(event.id, i)}>Delete</button>
		</div>
		<!-- <Item title={item} index={index} onEdit={onEdit} onRemove={onRemove} /> -->
	{/each}
</div>

<style>
	div {
		border-top: 3px solid lightcoral;
	}
</style>