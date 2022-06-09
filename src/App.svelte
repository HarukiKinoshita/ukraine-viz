<svelte:head>
	<link rel="stylesheet" href="https://unpkg.com/mono-icons@1.0.5/iconfont/icons.css" >
	<style>
		@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&display=swap');
	</style>
</svelte:head>


<script>
	import Scatterplot from './Scatterplot.svelte';
	import Map from './Map.svelte';
	import data from './data.js';
	import countryList from './country_list.js';
	import { timeParse } from 'd3-time-format';

	let count_by_country = {};
	data.meeting.forEach(d => {
    d.date = timeParse("%Y/%m/%d")(d.date);
		d.date = new Date(d.date);
		d.y = countryList.findIndex(el => el.country == d.country);
		if (data.type !== "speech") count_by_country[d.country] ? count_by_country[d.country] += 1 : count_by_country[d.country] = 1;
  })

	data.event.forEach(d => {
    d.date = timeParse("%Y/%m/%d")(d.date);
		d.date = new Date(d.date);
		d.y = countryList.findIndex(el => el.country == d.country);
  })
</script>

<div class="chart">
	<h1 class="title">Zelensky and World Leaders</h1>
	<p class="subtitle">President Zelensky Works in Close Contact with World Leaders</p>
	<Scatterplot points={data.meeting} ePoints={data.event} count_by_country={count_by_country} countryList={countryList} />
	<div style="text-align: center;">
		<Map countryList={countryList} />
	</div>
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
		max-width: 1280px;
		height: calc(100% - 4em);
		min-height: 480px;
		max-height: 640px;
		margin: 0 auto;
	}
</style>