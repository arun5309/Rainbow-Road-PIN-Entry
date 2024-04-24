<script lang="ts">
	import { colors, colorsNames, getPermutationMatrix, random1toNOrder } from './utils';
	import { createEventDispatcher } from 'svelte';
	import KeyPad from "./KeyPad.svelte";
	const dispatch = createEventDispatcher();

	function pickRoad(c: number, v: Array<Array<number>>) {
		dispatch('pickRoad', {
			color: c,
			value: v
		});
	}

	// export let rounds: number;
	export let numColors: number = 1;
	export let voicePromptedColor = 0;//Math.floor(Math.random()*numColors)
	export let pMatrix = getPermutationMatrix(numColors);

	let colorsUsed = colorsNames.slice(0, numColors);
	// In case you want to randomize the order of the 'colors' uncomment below line and use 'colorPresentationOrder[cycle]' instead of just 'cycle'
	// let colorPresentationOrder = random1toNOrder(numColors);
	let cycle = 0;
	let cycleInterval = setInterval(()=>{ cycle = (cycle + 1)%numColors },800);
	// function cyclePermutations(){
	// 	return pMatrix[Math.floor(cycle/10)]
	// }
</script>

<table id="rainbowRoad">
	<thead>
		<!-- Define headers
        1st and Last columns are for user reference
        Middle Column Titles rendered to match the color order of the permutations in the body -->
		<tr>
			<!-- <th style="min-width:30vw"><strong>Your</strong> Digit</th> -->
			<!-- eslint-disable-next-line @typescript-eslint/no-unused-vars -->
			{#each [0,1,2,3,4,5,6,7,8,9] as digit}
				<!-- <th
					style="background-color:{colors[colorPresentationOrder[i]]};
				min-width:{50 / numColors}vw"
				>
					{colorsNames[colorPresentationOrder[i]]}
				</th> -->
				<th style="background-color:#ffffff">{digit}</th>
			{/each}
		</tr>
	</thead>
	<tbody id="tableBody">
		<!-- For each digit (0-9) enumerate user options
        then present 'numColors' number of permutations in a random order -->
		<tr>
			{#each [0,1,2,3,4,5,6,7,8,9] as digit}
			<th style="background-color:#dedede">{colorsUsed[cycle] + String(pMatrix[cycle][digit])}</th>
			{/each}
		<!-- {#each pMatrix as _,i} -->
		<!-- <tr> -->
				<!-- eslint-disable-next-line @typescript-eslint/no-unused-vars -->
				<!-- {#each [0,1,2,3,4,5,6,7,8,9] as digit} -->
					<!-- <th
						style="background-color:{colors[colorPresentationOrder[i]]}; 
                    "
					>
						{pMatrix[colorPresentationOrder[i]][digit]}
					</th> -->
					<!-- <th id = {String(digit)} style="background-color:#aaaaaa"> -->
						<!-- {cyclePermutations()[digit]} -->
						<!-- {pMatrix[colorPresentationOrder[i]][digit]} -->
					<!-- </th> -->
				<!-- {/each} -->
		<!-- </tr> -->
		<!-- {/each} -->
		</tr>
	</tbody>
</table>
<br />

<!-- Allow user to select a permutation (or Road). 
    the name of this permutation is the color
    colors are represented as integers for parameterizability -->
<div>
	<!-- eslint-disable-next-line @typescript-eslint/no-unused-vars -->
	<!-- Pick the {colorsUsed[voicePromptedColor]} road -->
	<!-- {#each colorsUsed as _, i}
		<button
			class="roads"
			style="background-color:{colors[
				colorPresentationOrder[i]
			]}; align-items:center;justify-content:center;"
			on:click={() => pickRoad(colorPresentationOrder[i], pMatrix[colorPresentationOrder[i]])}
		>
			{colorsNames[colorPresentationOrder[i]]} Code
		</button>
	{/each} -->
</div>

<style>
	table {
		width: 100%;
		border-style: solid;
		border-color: black;
		border-collapse: collapse;
		font-size: 1.3em;
	}
	th {
		border-style: solid;
		border-color: black;
	}
	.roads {
		/* display: grid;
		grid-template-columns: repeat(1, 1fr); */
		min-width: 30vw;
		min-height: 7vh;
		border-style: solid;
		border-color: black;
		font-size: 1em;
		text-align: center;
	}
</style>
