<svelte:head>
	<style>
		@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&display=swap');
	</style>
</svelte:head>


<script>
	import { getContext, setContext } from 'svelte';
	import Scatterplot from './Scatterplot.svelte';
	import Map from './Map.svelte';
	import data from './data.js';
	import countryList from './country_list.js';
	import { timeParse } from 'd3-time-format';
	import Scroller from "@sveltejs/svelte-scroller";

  let index, offset, progress;

	// COLOR THEME
	const darkModeMediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
	const isDarkMode = darkModeMediaQuery.matches;
	setContext("theme", isDarkMode ? "dark" : "light");
	console.log(getContext("theme"));

	// GET WIDTH AND HEIGHT OF CHILD COMPONENT
	// let scatterplot_size = { width: 0, height: 800 };
	// function updateChild(event) {
	// 	scatterplot_size.width = event.detail.width;
	// 	scatterplot_size.height = event.detail.height;
  //   console.log(event.detail.width, event.detail.height);
  // }

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

<div class="top" style="text-align: center;">
	<h1 class="title">How Ukraine Has Gained Momentum</h1>
	<p class="subtitle">Casting a Spotlight on Ukraine's Victory on the World Stage</p>
</div>

<div class="chart">
	<h2 class="headline">Eight years ago, and Now</h2>
	<div style="text-align: center;">
		<Map countryList={countryList} />
	</div>
</div>

<Scroller top='{0}' bottom='{1}' bind:index bind:offset bind:progress>
  <div slot="background" class="scroller-background-container">
    <h2 class="headline">It all starts with saying "Hello?"</h2>
		<div class="scatterplot_wrapper">
			<Scatterplot points={data.meeting} ePoints={data.event} count_by_country={count_by_country} countryList={countryList} style="position: relative;" />
		</div>

    <p>Section {index + 1} is currently active.</p>
  </div>

  <div slot="foreground">
    <section>
			<div class="inner">
				United States determined to give
			</div>
		</section>
    <section>
			<div class="inner">
				United States determined to give
			</div>
		</section>
    <section>
			<div class="inner">
				United States determined to give
			</div>
		</section>
  </div>
</Scroller>

<style>
	.title {
		font-family: 'Playfair Display', serif;
		margin-bottom: 4px;
		font-size: 3.5rem;
	}
	.subtitle {
		font-family: 'Playfair Display', serif;
		font-size: 1.2rem;
		color: #94A3B8;
	}

	.headline {
		font-family: 'Playfair Display', serif;
		margin: 4vh 0 4vh 0;
		font-style: italic;
		font-size: 2.5rem;
		width: 25vw;
		min-width: 360px;
		text-decoration: underline;
		text-decoration-color: #94A3B8;
	}
	.chart {
		width: 100%;
		max-width: 1280px;
		min-height: 640px;
		/* max-height: 640px; */
		margin: 0 auto;
	}
	.scatterplot_wrapper {
		height: 75vh;
	}
	section {
    height: 80vh;
		position: relative;
  }
	.inner{
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translateY(-50%) translateX(-50%);
		-webkit-transform: translateY(-50%) translateX(-50%);
		padding: 5vw;
		background-color: white;
		box-shadow: rgba(0, 0, 0, 0.16) 0px 10px 36px 0px, rgba(0, 0, 0, 0.06) 0px 0px 0px 1px;
	}
	.scroller-background-container {
		pointer-events: all !important;
	}
	/* div {
		border: 1px solid lightgray;
	} */
</style>