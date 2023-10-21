<script>
	import { onMount } from 'svelte'

	import Calendar from '@event-calendar/core'
	import TimeGrid from '@event-calendar/time-grid'
	import DayGrid from '@event-calendar/day-grid'
	import List from '@event-calendar/list'
	// import Interaction from '@event-calendar/interaction'

	onMount(() => {
		const button = document.querySelector('.ec-toolbar .ec-today')
		button.style.color = '#212529'
		// button.style.display = 'inline-block'
		// button.style.removeProperty('width')
		// button.style.width = '5rem'
	})

	let ec

	let plugins = [TimeGrid, DayGrid, List]

	let options = {
		view: 'dayGridMonth',
		slotDuration: '01:00',
		firstDay: 1,
		headerToolbar: {
			start: 'prev,next,today',
			center: 'title',
			end: 'dayGridMonth,timeGridWeek,timeGridDay,listMonth'
		},
		eventSources: [{ url: 'http://localhost:8180/events' }]
	}

	function invokeMethod() {
		ec.refetchEvents()
	}

	function updateOptions() {
		options.slotDuration = options.slotDuration === '01:00' ? '00:30:00' : '01:00'
	}
</script>

<div class="grid">
	<button on:click={invokeMethod}>Refetch events</button>

	<button on:click={updateOptions}>Change slot duration</button>
</div>

<Calendar bind:this={ec} {plugins} {options} />
