<script>
	export let showModal; 
	import close from "$lib/gallery/close_icon.png";
    import icon from "$lib/gallery/icon.png";

	let dialog; 

	$: if (dialog && showModal) dialog.showModal();
</script>

<dialog
	bind:this={dialog}
	on:close={() => (showModal = false)}
	on:click|self={() => dialog.close()}
>
	<div on:click|stopPropagation>
		<button class="button" autofocus on:click={() => dialog.close()}><img src={close} alt="close" class="close"></button>
        <div class="centerDiv">
            <img src={icon} alt="icon">
		    <slot name="header" class="header" />
            <slot name="button" class="btn"></slot>
        </div>
	</div>
</dialog>

<style>
	dialog {
		width: 20em;
		height: 17em;
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
	.button {
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
    .centerDiv{
        text-align: center;
    }
    @media screen and (max-width: 700px) {
        dialog{
            margin-right: 6em;
        }
    }
</style>
