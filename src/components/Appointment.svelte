<script>
    
    let eventName = "noup";
    let timeHour = "14:50";

</script>


<tr>
	<td>{eventName}</td>
	<td>{timeHour}</td>	
</tr>


<style>
	td {
		text-align: left;
		padding: 6px;		
	}
	
	td,
	input {
		cursor: pointer
	}
	
	tr:nth-child(even) {
		background-color: #f2f2f2;
	}
	
	input[type="checkbox"] {
		width: 55px;
		height: 25px;
		margin-top: 10px;
	}

	input[type=checkbox]:checked {
		background-color: hsl(168, 76%, 100%);
		color: white;
	}
	
	.completed {
		text-decoration: line-through;
		color: #aaa;
	}	
</style>