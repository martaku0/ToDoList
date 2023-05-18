<script>
	import TodoList from '@/lib/TodoList.svelte';
    import EventPropertyInputs from './lib/EventPropertyInputs.svelte';
	import Timer from './lib/Timer.svelte';

	let events = [];

	let numOfDoneEventsBasedByTime = 0;

	const addEvent = (event) => {
		event.id = Math.floor(Date.now() * Math.random());

		const timeBetween = event.reminderTimestamp - Date.now();

		event.reminderTimeout = setTimeout(() => {
			NotificationUtils.showNotification(event.name, event.message);
			event.expired = true;
			events = events;
		}, timeBetween);

		events = [...events, event];
	};

	const deleteEvent = (id, index) => {
		const deletedEvent = events.find(event => event.id === id);
		clearTimeout(deletedEvent.reminderTimeout);
		events = events.filter(event => event.id !== id);
		if(index < numOfDoneEventsBasedByTime){
			numOfDoneEventsBasedByTime -= 1;
		}
	}

	const changeEvents = (num) => {
		numOfDoneEventsBasedByTime = num;
	}
</script>

<main>
	<Timer handleChangeEvents={changeEvents} events={events} numOfDoneEtc={numOfDoneEventsBasedByTime}/>
	<EventPropertyInputs handleAddEvent="{addEvent}" />
	<TodoList events={events} handleDeleteEvent={deleteEvent} numOfDoneEventsBasedByTimeButInTodoListLib={numOfDoneEventsBasedByTime}/>
</main>

<style>
	main {
		background-color: lightblue;
		margin: 0;
		padding: 15px;
		min-height: calc(100vh - 30px);
	}
	:global(body) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}
</style>
