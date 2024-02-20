<script lang='ts'>
	import KeyPad from "./KeyPad.svelte";
	import RainbowRoad from "./RainbowRoad.svelte";
	import {colors} from './utils'
	let show_keypad_screen = false;
	let show_rainbow_screen = true;

	let value = '';
	let rounds = 0;
	let userEnteredPIN = '';

	// from KeyPad
	let keyPadColor:number = 4;

	// from RainbowRoad
	let numColors:number = 4;

	function handleNumKeyPress(event:CustomEvent){
		let num = event.detail.value;
		if(value.length > 0){
			console.log("no more input allowed")
		} else {
			value = value.concat(num)
		}
	}
	function handleActionKeyPress(event:CustomEvent){
		let key = event.detail.value;
		if(key === 'backspace'){
			value = value.substring(0,value.length-1);
		} else if (key === 'enter'){
			console.log("next round");
			userEnteredPIN = userEnteredPIN.concat();
		} else {
			console.log("something other than 'backspace' or 'enter' was sent")
		}
	}

</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	{#if show_keypad_screen}
	<h1>Rainbow Road PIN Entry</h1>
	<input id="PIN_Entry" bind:value /><br>
	<KeyPad {keyPadColor} on:numKey={handleNumKeyPress} on:actionKey={handleActionKeyPress}/>
	{/if}

	{#if show_rainbow_screen}
	<RainbowRoad rounds={4} numColors={numColors}/>
	{/if}
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	#PIN_Entry {
		min-width: 80vw;
	}
</style>
