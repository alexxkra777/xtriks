<script>
	const date = new Date();
	
	const today = {
		dayNumber: date.getDate(),
		month: date.getMonth(),
		year: date.getFullYear(),
	}
	
	const monthNames = [ "Leden", "Únor", "Březen", "Duben", "Květen", "Červen", "Červenec", "Srpen", "Září", "Říjen", "Listopad", "Prosinec"];
	let monthIndex = date.getMonth();
	// const currentMonth = date.toLocaleString('en-US', { month: 'long' })
	$: month = monthNames[monthIndex];
	
	let year = date.getFullYear();
	
	$: firstDayIndex = new Date(year, monthIndex, 1).getDay();
	// const currentDay = date.getDate();
	$: numberOfDays = new Date(year, monthIndex+1, 0).getDate();
	
	$: calendarCellsQty = firstDayIndex <= 4 ? 35 : 42;	
	
	const goToNextMonth = () => {
		if (monthIndex >= 11) {
			year += 1;
			return monthIndex = 0;
		}
		return monthIndex += 1;
	}
	
	const goToPrevMonth = () => {
		if (monthIndex <= 0) {
			year -= 1;
			return monthIndex = 11;
		}
		return monthIndex -= 1;
	}
	
// 	$: console.log(`${month}, ${today.dayNumber}, ${year}, FIRST DAY index is ${firstDayIndex}, MONTH index is ${monthIndex}, No. of days: ${numberOfDays}`)
</script>


	<div class="month">
		<ul>
			<li class="prev" on:click={goToPrevMonth}>&#10094;</li>
			<li class="next" on:click={goToNextMonth}>&#10095;</li>
			<li>{month}<br>
				<span style="font-size:18px">{year}</span>
			</li>
		</ul>
	</div>

	<ul class="weekdays">
		<li>Ne</li>
		<li>Po</li>
		<li>Út</li>
		<li>St</li>
		<li>Čt</li>
		<li>Pá</li>
		<li>So</li>
	</ul>

	<ul class="days">
		{#each Array(calendarCellsQty) as _, i}
			{#if i < firstDayIndex || i >= numberOfDays+firstDayIndex  }
				<li>&nbsp;</li>
			{:else}
				<li class:active={i === today.dayNumber+(firstDayIndex-1) &&
													monthIndex === today.month &&
													year === today.year}
						data-dateID={`${month}_${(i-firstDayIndex)+1}_${year}`}
						class:has-appts={`${month}_${(i-firstDayIndex)+1}_${year}`}
						on:click>
					{(i - firstDayIndex) + 1}
				</li>
			{/if}
		{/each}
	</ul>


				
<style>
	ul {list-style-type: none;}

	/* Month header */
	.month {
		padding: 7% 25%;
		width: auto;
		background: white;
		text-align: center;
	}

	/* Month list */
	.month ul {
		margin: 0;
		padding: 0;
	}

	.month ul li {
		color: #83464F;
		font-size: 20px;
		text-transform: uppercase;
		letter-spacing: 3px;
	}

	/* Previous button inside month header */
	.month .prev {
		float: left;
		padding-top: 10px;
		cursor: pointer;
	}

	/* Next button */
	.month .next {
		float: right;
		padding-top: 10px;
		cursor: pointer;
	}

	/* Weekdays (Mon-Sun) */
	.weekdays {
		margin: 0;
		padding: 10px 0;
		background-color:#e5d5d1;
	}

	.weekdays li {
		display: inline-block;
		width: 13.6%;
		color: #83464F;
		text-align: center;
	}

	/* Days (1-31) */
	.days {
		padding: 10px 0;
		background: white;
		margin: 0;
		position: relative;
    	text-align: center;
	}

	.days li {
		list-style-type: none;
		display: inline-block;
		border: 2px solid #83464F;
		padding: 35px 10px;
		width: 11.6%;
		text-align: center;
		font-size: 1.2rem;
		color: #777;
		cursor: pointer;
	}

	/* Highlight the "current" day */
	.active {
		padding: 5px;
		background: #e5d5d1;
		color: white;
	}
	
	.days li.has-appts {
		color: #83464F;
	}
	@media screen and (max-width: 1400px) {
		.days li {
			padding: 30px 5px;
		}
	}
	@media screen and (max-width: 700px) {
		.weekdays{
			text-align: center;
		}
		.weekdays li {
			width: 10.6%;
		}
		.days li {
			padding: 30px 10px;
		}
	}
</style>