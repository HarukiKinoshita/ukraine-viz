<script>
	import { onMount } from 'svelte';
  import { scaleLinear, timeParse, timeFormat, timeDay, extent, scaleTime } from 'd3';
  // import { scaleLinear, scaleTime } from 'd3-scale';
  // import { timeParse, timeFormat } from 'd3-time-format';
  import countryList from './country_list.js';

	export let mPoints;
	// export let ePoints;
	export let count_by_country;

	let svg;
	let width = 500;
	let height = 300;

	const padding = { top: 20, right: 80, bottom: 40, left: 150 };

  let extentX = extent(mPoints, (d) => d.date);

	$: xScale = scaleTime()
		.domain(extentX)
		.range([padding.left, width - padding.right]);

	$: yScale = scaleLinear()
		.domain([-2, countryList.length-1])
		.range([height - padding.bottom, padding.top]);

	// $: xTicks = width > 180 ?
  //   [new Date("2022-02-24"), new Date("2022-03-24"), new Date("2022-04-24"), new Date("2022-05-24"), new Date("2022-06-24")] :
  //   [new Date("2022-02-24"), new Date("2022-04-24"), new Date("2022-06-24")]
		// [0, 4, 8, 12, 16, 20] :
		// [0, 10, 20];

	// $: yTicks = height > 180 ?
	// 	[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20] :
	// 	[0, 4, 8, 12];

	onMount(resize);

	function resize() {
		({ width, height } = svg.getBoundingClientRect());
	}
	function handleMouseOver(e) {
		console.log(e.target.getAttribute("country"));
	}
</script>

<svelte:window on:resize='{resize}'/>

<svg bind:this={svg}>
	<!-- y axis -->
	<g class='axis y-axis'>
		<!-- {#each yTicks as tick}
			<g class='tick tick-{tick}' transform='translate(0, {yScale(tick)})'>
				<line x1='{padding.left}' x2='{xScale(new Date("2022-06-24"))}'/>
				<text x='{padding.left - 16}' y='+4'>{countryList[tick] ? countryList[tick].country : ""}</text>
			</g>
		{/each} -->
		{#each countryList as tick, index}
			<g class='tick tick-{index}' transform='translate(0, {yScale(index)})'>
				<line x1='{padding.left - 18}' x2='{width - padding.right}'/>
				<text x='{padding.left - 56}' y='+4'>{tick.country}</text>
				{#if tick.nato}
        <circle
          cx='{padding.left - 44}'
          r='3'
          fill='#4281d4'
        />
        {/if}
				{#if tick.eu}
        <circle
          cx='{padding.left - 32}'
          r='3'
          fill='#f7c80c'
        />
        {/if}
			</g>
		{/each}
	</g>

	<!-- x axis -->
	<g class='axis x-axis'>
		{#each xScale.ticks(7) as tick}
			<g class='tick' transform='translate({xScale(tick)},0)'>
				<line y1='{yScale(0) + 18}' y2='{yScale(countryList.length-1)}'/>
				<text y='{height - padding.bottom - 14}'>{timeFormat("%b %d")(tick)}</text>
			</g>
		{/each}
	</g>

	<!-- data -->
	<g class="data">
		{#each mPoints.filter(el => el.y !== -1) as point}
			<!-- <Emoji
				x='{xScale(point.date)}'
				y='{yScale(point.y)}'
				code="es"
			/> -->
			<circle
				class="meeting"
				country='{point.country}'
				cx='{xScale(point.date)}'
				cy='{yScale(point.y)}'
				r='5'
				fill='{point.place == 'In-Person' ? 'orange' : 'lightgray'}'
			>
				<title>{point.country}</title>
			</circle>
		{/each}
	</g>

	<g class="bars">
		{#each mPoints.filter(el => el.y !== -1) as point, i}
			<circle
				cx="{width - padding.right + 40}"
				cy="{yScale(point.y)}"
				r="{count_by_country[point.country]*0.8}"
				fill="#cae8e2"
				fill-opacity=0.6
				stroke="#8dcec1"
			/>
			{#if count_by_country[point.country] >= 10}
			<text
				text-anchor="middle"
				x="{width - padding.right + 40}"
				y="{yScale(point.y) + 4}"
			>
				{count_by_country[point.country]}
			</text>
			{/if}
			<!-- <rect
				x="{width - padding.right + 20}"
				y="{yScale(point.y)-4}"
				height="{8}"
				width="{count_by_country[point.country]*3}"
				fill="#CBD5E1"
			/>
			-->
		{/each}
	</g>
	<!-- data -->
	<!-- {#each ePoints.filter(el => el.y !== -1) as point}
    <circle
      cx='{xScale(point.date)}'
      cy='{yScale(point.y)}'
      r='3'
      fill='black'
      fill-opacity='0.6'
    />
	{/each} -->
</svg>

<style>
	svg {
		width: 100%;
		height: 100%;
		float: left;
	}

	circle.meeting {
		/* fill: orange; */
		fill-opacity: 0.6;
		stroke: rgba(0,0,0,0.5);
	}

	.x-axis .tick line {
		stroke: #eee;
		stroke-dasharray: 2;
	}

	.y-axis .tick line {
		stroke: #ddd;
	}

	text {
		font-size: 12px;
		fill: #999;
	}

	.x-axis text {
		text-anchor: middle;
	}

	.y-axis text {
		text-anchor: end;
	}
</style>