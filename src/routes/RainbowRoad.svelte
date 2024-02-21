<script lang='ts'>
    import { colors, colorsNames, shuffle, getPermutationMatrix, random1toNOrder, gameHeader } from './utils'
    import {createEventDispatcher } from 'svelte'
    const dispatch = createEventDispatcher();

    function pickRoad(c:number,v:Array<number>){
        dispatch('pickRoad',{
            color:c,
            value:v
        })
    }

    export let rounds:number;
    export let numColors:number =1;

    let colorsUsed = colorsNames.slice(0,numColors);
    let pMatrix = getPermutationMatrix(numColors);
    let colorPresentationOrder = random1toNOrder(numColors);

</script>

<table id="rainbowRoad">
    <thead>
        <!-- Define headers
        1st and Last columns are for user reference
        Middle Column Titles rendered to match the color order of the permutations in the body -->
        <tr>
           <th>{gameHeader(rounds)} Digit</th>
           {#each colorsUsed as c,i}
           <th style="background-color:{colors[colorPresentationOrder[i]]}">
                {colorsNames[colorPresentationOrder[i]]} Partners
            </th>
           {/each}
           <th>{gameHeader(rounds)} Digit</th>
        </tr>
    </thead>
    <tbody id="tableBody">
        <!-- For each digit (0-9) enumerate user options
        then present 'numColors' number of permutations in a random order -->
        {#each [0,1,2,3,4,5,6,7,8,9] as digit}
        <tr>
            <th>{digit} &#62</th>
            {#each pMatrix as perm,i}
            <th style="background-color:{colors[colorPresentationOrder[i]]}; 
                    min-width:{100/(numColors+2)}vw;"
            >
                {pMatrix[colorPresentationOrder[i]][digit]}
            </th>
            {/each}
            <th>&#60 {digit}</th>
        </tr>
        {/each}
    </tbody>
</table>
<br>
<!-- Allow user to select a permutation (or Road). 
    the name of this permutation is the color
    colors are represented as integers for parameterizability -->
{#each colorsUsed as c,i}
<button 
    class="roads" 
    style="grid-auto-rows:minmax({30/numColors}vh,auto);
    background-color:{colors[colorPresentationOrder[i]]}"
    on:click = {() => pickRoad(colorPresentationOrder[i],pMatrix[colorPresentationOrder[i]])}
    >   
    {colorsNames[colorPresentationOrder[i]]} Partner
</button>
{/each}

<style>
    table {
        width: 100%;
    }
    .roads {
        display: grid;
		grid-template-columns: repeat(1, 1fr);
		min-width: 75vw;
		border-style: solid;
        border-color: black;
        font-size: 1.5em;
        text-align: center;
    }

</style>