<script lang='ts'>
	import StartPage from "./StartPage.svelte";
	import KeyPad from "./KeyPad.svelte";
	import RainbowRoad from "./RainbowRoad.svelte";
	import EndPage from "./EndPage.svelte";
	import {colors, colorsNames, gameHeader} from './utils'
	
	// Initial game state values
	let showStartScreen = true;
	let showKeypadScreen = false;
	let showRainbowScreen = false;
	let showEndScreen = false;
	let showTutorialElements = true;
	let value = '';
	let permutation:Array<number> = []; 
	let rounds = 0;
	let userEnteredPIN = '';

	// Phase Staging
	function stageStartScreen(){
		showStartScreen = true;
		showKeypadScreen = false;
		showRainbowScreen = false;
		showEndScreen = false;
	}
	function stageKeyPadScreen(){
		showStartScreen = false;
		showKeypadScreen = true;
		showRainbowScreen = false;
		showEndScreen = false;
	}
	function stageRainbowScreen(){
		showStartScreen = false;
		showKeypadScreen = false;
		showRainbowScreen = true;
		showEndScreen = false;
	}
	function stageEndScreen(){
		showStartScreen = false;
		showKeypadScreen = false;
		showRainbowScreen = false;
		showEndScreen = true;
	}

	// from KeyPad
	let keyPadColor:number;
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
		} else if (key === 'enter' && value.length > 0){
			let mappedPINDigit = String(permutation.indexOf(Number(value)));
			userEnteredPIN = userEnteredPIN.concat(mappedPINDigit);
			permutation = [];
			value = '';
			rounds += 1;
			if (rounds >= 4){
				stageEndScreen();
			} else {
				stageRainbowScreen();
			}
		} else if (key === 'enter' && value.length === 0) {
			console.log("Ptp tried submitting blank")
		} else {
			console.log("Something other than 'backspace' or 'enter' was sent")
		}
	}

	// from RainbowRoad
	let numColors:number = 4;
	function handlePickRoad(event:CustomEvent){
		let colorIndex = event.detail.color;
		let perm = event.detail.value;

		keyPadColor = colorIndex;
		permutation = perm;

		console.log("Color Index of Road chosen: " + colorIndex)
		console.log("Permutation: " + perm)
		stageKeyPadScreen();
	}
	
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	{#if showStartScreen}
	<StartPage on:click={()=> stageRainbowScreen()} />
	{/if}

	{#if showKeypadScreen}
	<h2>Type the number from the {colorsNames[keyPadColor]} Road</h2>
	<input id="PIN_Entry" bind:value /><br>
	<KeyPad {keyPadColor} on:numKey={handleNumKeyPress} on:actionKey={handleActionKeyPress}/>
	{/if}

	{#if showRainbowScreen}
	{#if showTutorialElements}
	<h2><strong><u>{gameHeader(rounds+1)} digit</u></strong> of 4 on the margins. Remember one of your digit's partners on any Road you'd like.</h2>
	{:else}
	<h1><strong>{gameHeader(rounds+1)} digit</strong> of 4</h1>
	{/if}
	<RainbowRoad rounds={rounds+1} numColors={numColors} on:pickRoad={handlePickRoad}/>
	<h2>Choose a Road</h2>
	{/if}

	{#if showEndScreen}
	<EndPage PIN={userEnteredPIN}/>
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

	h2 {
		width: 100%;
		text-align: center;
	}

	#PIN_Entry {
		min-width: 80vw;
	}
</style>
