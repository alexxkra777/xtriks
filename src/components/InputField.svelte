<script>
	export let value, label, type = 'text';
	
	function typeAction(node){
		node.type = type;
	}
</script>

<p class="form-control">
	{#if label}
		<label class="label" for>{label}:</label>
	{/if}
    <input use:typeAction class="input" bind:value={value} required/>
</p>

<style>
	.form-control{
		margin: .5rem 0;
		text-align: left;
	}
    .input {
        width: 100%;
        display: block;
        margin-top: 1rem;
        padding: 0.5rem 0;
        border: 2px solid #83464F;
        border-radius: 10px;
        color: #83464F;
        background-color: #F7E8E9;
        box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
        transition: all 0.3s ease;
        padding-left: 1%;
    }

    .input:focus {
        outline: none;
        border-color: white;
        box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.2);
    }
    .label{
        color: #83464F;
    }
</style>