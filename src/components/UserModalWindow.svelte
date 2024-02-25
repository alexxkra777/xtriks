<script>
	export let showModal; 
	import icon from "$lib/gallery/close_icon.png";

	let dialog; 

	$: if (dialog && showModal) dialog.showModal();
</script>

<dialog
	bind:this={dialog}
	on:close={() => (showModal = false)}
	on:click|self={() => dialog.close()}
>
	<div on:click|stopPropagation>
		<button autofocus on:click={() => dialog.close()}><img src={icon} alt="icon" class="close"></button>
		<div class="grid-container">
			<div class="grid-item"><h3>Jméno a příjmení</h3><slot name="header" /></div>
			<div class="grid-item"><h3>Email</h3><slot name="email"/></div>
			<div class="grid-item"><h3>Podnikání</h3><slot name="bussines" /></div>
			<div class="grid-item"><h3>Adresa</h3><slot name="adress"/></div>
			<div class="grid-item"><h3>Město</h3><slot name="city"/></div>
		</div>
	</div>
</dialog>

<style>
	dialog {
		width: 40em;
		height: 45em;
		border-radius: 0.2em;
		border: none;
		padding: 0;
		background-color: #e5d5d1;
		border-radius: 20px;
	}
	dialog::backdrop {
		background: rgba(0, 0, 0, 0.3);
	}
	dialog > div {
		padding: 1em;
	}
	dialog[open] {
		animation: zoom 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
	}
	@keyframes zoom {
		from {
			transform: scale(0.95);
		}
		to {
			transform: scale(1);
		}
	}
	dialog[open]::backdrop {
		animation: fade 0.2s ease-out;
	}
	@keyframes fade {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}
	button {
		display: block;
		border: none;
		margin: 0;
		padding: 0;
		text-align: inherit;
		font: inherit;
		border-radius: 0;
		appearance: none; 
		background-color: #e5d5d1;
	}
	.close{
		width: 40px;
		height: 40px;
		cursor: pointer;
	}
	.grid-container {
		margin-left: 7em;
    	display: grid;
    	grid-template-columns: auto auto;
    	border: 1px solid #e5d5d1;
    	padding: 10px;
  	}
	.grid-item {
   	 	border: 1px solid #e5d5d1;
    	padding: 8px;
  	}
	h3{
		font-size: 14px;
	}
	  @media screen and (max-width: 600px) {
		.grid-container{
			margin-left: 3em;
		}
	  }
</style>
