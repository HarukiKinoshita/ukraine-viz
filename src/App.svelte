<svelte:head>
	<style>
		@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&display=swap');
	</style>
</svelte:head>


<script>
	import Scatterplot from './Scatterplot.svelte';
	import data from './data.js';
	import countryList from './country_list.js';
	import { timeParse } from 'd3-time-format';


	data.meeting.forEach(d => {
    d.date = timeParse("%Y/%m/%d")(d.date);
		d.date = new Date(d.date);
		d.y = countryList.findIndex(el => el.country == d.country);
		// console.log(d.country, d.y);
  })

	data.event.forEach(d => {
    d.date = timeParse("%Y/%m/%d")(d.date);
		d.date = new Date(d.date);
		d.y = countryList.findIndex(el => el.country == d.country);
		// console.log(d.country, d.y);
  })

</script>

<div class="chart">
	<h1 class="title">Talks with Zelensky</h1>
	<p class="subtitle">President Zelensky Works in Close Contact with World Leaders</p>

	<Scatterplot mPoints={data.meeting} ePoints={data.event} />
</div>

<style>
	.title {
		font-family: 'Playfair Display', serif;
		margin-bottom: 4px;
		font-size: 3rem;
	}
	.subtitle {
		font-family: 'Playfair Display', serif;
		margin-top: 0;
		font-size: 1.4rem;
		color: #94A3B8;
	}
	.chart {
		width: 100%;
		max-width: 960px;
		height: calc(100% - 4em);
		min-height: 480px;
		max-height: 640px;
		margin: 0 auto;
	}
</style>