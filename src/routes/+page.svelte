<script lang="ts">
	import { onMount } from 'svelte';

	let timeSlots: string[] = [];
	let taskContainers: string[] = [];
	let startTime: string = '06:00';
	let endTime: string = '21:00';
	let color: string;

	const onDrag = (e: DragEvent) => {
		(e.target as HTMLDivElement).classList.forEach((colorClass) => {
			if (colorClass.includes('bg-')) color = colorClass;
		});
	};

	const onDrop = (e: DragEvent) => {
		e.preventDefault();
		(e.target as HTMLDivElement).setAttribute('class', `w-full p-2 ${color}`);
	};

	const getTimes = (start: string, finish: string): string[] => {
		let startDateTime = new Date('1970-01-01T' + start + 'Z');
		let finishDateTime = new Date('1970-01-01T' + finish + 'Z');

		let arr: string[] = [];
		arr.push(start);

		while (startDateTime < finishDateTime) {
			startDateTime.setMinutes(startDateTime.getMinutes() + 30);

			arr.push(startDateTime.toISOString().substring(11, 16));
		}

		return arr;
	};

	$: startTime && endTime ? (taskContainers = getTimes(startTime, endTime)) : null;

	onMount(() => {
		timeSlots = getTimes('00:30', '24:00');
	});
</script>

<div class="grid grid-cols-2">
	<div class="grid-flow-row">
		{#each taskContainers as taskContainer}
			<div class="grid grid-cols-2 border-b-2 border-b-white">
				<div class="text-center">
					<p>
						{taskContainer}
					</p>
				</div>
				<div
					on:drop={onDrop}
					on:dragover={(e) => e.preventDefault()}
					on:dragenter={(e) => e.preventDefault()}
					class="w-full bg-slate-400 p-2"
				/>
			</div>
		{/each}
	</div>
	<div>
		<div class="text-center m-2">
			<div class="">
				<select class="border-2 border-blue-400 rounded-lg" bind:value={startTime}>
					<option value="" disabled selected>Start tid</option>
					{#each timeSlots as timeSlot}
						<option value={timeSlot}>{timeSlot}</option>
					{/each}
				</select>
				<select class="border-2 border-blue-400 rounded-lg" bind:value={endTime}>
					<option value="" disabled selected>Slut tid</option>
					{#each timeSlots as timeSlot}
						<option value={timeSlot}>{timeSlot}</option>
					{/each}
				</select>
			</div>
			<div class="grid grid-cols-3 m-2 justify-items-center">
				<div on:drag={onDrag} draggable="true" class="bg-blue-500 w-2/4 p-2 mb-2" />
				<div on:drag={onDrag} draggable="true" class="bg-green-500 w-2/4 p-2 mb-2" />
				<div on:drag={onDrag} draggable="true" class="bg-yellow-500 w-2/4 p-2 mb-2" />
				<div on:drag={onDrag} draggable="true" class="bg-red-500 w-2/4 p-2 mb-2" />
			</div>
		</div>
	</div>
</div>
