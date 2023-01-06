<script lang="ts">
	import { onMount } from 'svelte';

	let timeSlots: string[] = [];
	let taskContainers: string[] = [];
	let startTime: string;
	let endTime: string;

	const generateTimeslots = () => {};

	const getTimes = (start: string, finish: string): string[] => {
		let startDateTime = new Date('1970-01-01T' + start + 'Z');
		let finishDateTime = new Date('1970-01-01T' + finish + 'Z');

		let arr: string[] = [];
		arr.push(start);

		while (startDateTime < finishDateTime) {
			startDateTime.setMinutes(startDateTime.getMinutes() + 30);

			arr.push(startDateTime.toISOString().substr(11, 5));
		}

		return arr;
	};

	$: startTime && endTime ? (taskContainers = getTimes(startTime, endTime)) : null;

	onMount(() => {
		timeSlots = getTimes('00:30', '24:00');
	});
</script>

<div class="text-center m-2">
	<div class="">
		<select bind:value={startTime}>
			{#each timeSlots as timeSlot}
				<option value={timeSlot}>{timeSlot}</option>
			{/each}
		</select>
		<select bind:value={endTime}>
			{#each timeSlots as timeSlot}
				<option value={timeSlot}>{timeSlot}</option>
			{/each}
		</select>
	</div>
</div>
<div class="grid grid-flow-col">
	{#each taskContainers as taskContainer}
		<div>
			<div class="text-center pt-4 border-b-2">
				<p class="border-r-2">
					{taskContainer}
				</p>
			</div>
		</div>
	{/each}
</div>
