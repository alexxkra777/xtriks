<script>
	import Form from '../../components/Form.svelte';
	import ProgressBar from '../../components/ProgressBar.svelte';
  import BackArrow from "../../components/BackArrow.svelte";
	let steps = ['Info', 'Adresa', 'Registrace'], currentActive = 1, progressBar;
	
	const handleProgress = (stepIncrement) => {
		progressBar.handleProgress(stepIncrement)
	}
	
</script>
<BackArrow href="../../"></BackArrow>
<main>

	<div class="container">
		<ProgressBar {steps} bind:currentActive bind:this={progressBar}/>
		
		<Form active_step={steps[currentActive-1]}/>

		<div class="step-button">
			<button class="btn" on:click={() => handleProgress(-1)} disabled={currentActive == 1}>Zpátky</button>
			<button class="btn" on:click={() => handleProgress(+1)} disabled={currentActive == steps.length}>Dále</button>
		</div>		
	</div>	  
</main>

<style>

	@import url('https://fonts.googleapis.com/css?family=Muli&display=swap');
	
	* {
		box-sizing: border-box;
    font-family: sans-serif;
	}

	main {
		font-family: 'Muli', sans-serif;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 100vh;
		overflow: hidden;
		margin: 0;
	}

	.btn {
		background-color: #83464F;
		color: #e5d5d1;
		border: 0;
		border-radius: 6px;
		cursor: pointer;
		font-family: inherit;
		padding: 8px 30px;
		margin: 5px;
		font-size: 14px;
	}

	.btn:active {
		transform: scale(0.98);
	}

	.btn:focus {
		outline: 0;
	}

	.btn:disabled {
		background-color: #e5d5d1;
		cursor: not-allowed;
    color: #fff;
	}
	
	.step-button{
		margin-top: 1rem;
		text-align: center;
	}
</style>