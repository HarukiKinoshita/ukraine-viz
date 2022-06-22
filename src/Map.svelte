
<script>
  import { geoAlbers, geoNaturalEarth1, geoConicEqualArea, geoPath } from "d3-geo";
  import { onMount } from "svelte";
  import { feature } from "topojson";
  import voting from './voting.js';

  // export let countryList;
  let selected;
  let features = [];

  const projection = geoNaturalEarth1();
  const path = geoPath().projection(projection);

  onMount(async () => {
    const response = await fetch(
    "https://cdn.jsdelivr.net/npm/world-atlas@2/countries-50m.json"
    ).then(d => d.json())
    features = feature(response, response.objects.countries).features;
  });

  $: currentNum = 0;

  // $: vueltos = `M${countryList.map(p => `${projection(p.location)}`).join('L')}`;
  // $: vueltos = `M${data.meeting.map(p => `${projection(countryList.find(el => el.country.includes(p.country)).location)}`).join('L')}`;

  $: color_approve = "#0D9488"
  $: color_against = "#E11D48"
  $: color_abstain = "#CA8A04"
  $: color_absent = "#475569"

  function selectColor(name, currentNum) {
    if (voting[currentNum].approve.find(el => el == name)) return color_approve
    else if (voting[currentNum].against.find(el => el == name)) return color_against
    else if (voting[currentNum].abstain.find(el => el == name)) return color_abstain
    else if (voting[currentNum].absent.find(el => el == name)) return color_absent
    else return "rgba(255,255,255,0.2)"
    // https://tailwindcss.com/docs/customizing-colors
  }
</script>

<style>
  .border {
    stroke: #ddd;
    stroke-width: 0.4;
  }
  .bar-item {
    padding: .2rem 0;
    font-weight: bold;
  }
  button {
    background: none;
    border: none;
		border-bottom: 5px solid #94A3B8;
		border-radius: 0;
    cursor: pointer;
    font-size: 1.5rem;
    padding-left: 32px;
    padding-right: 32px;
    opacity: 0.5;
  }
  .selected {
    opacity: 1;
    border-bottom: 5px solid #2DD4BF;
    font-weight: bold;
  }
  @media (prefers-color-scheme: dark) {
    .border {
      stroke: #041a28;
    }
    button {
      color: #fff;
    }
  }
</style>

<div>
  <button on:click={() => {currentNum = 0}} class:selected="{currentNum == 0}">2014</button>
  <button on:click={() => {currentNum = 1}} class:selected="{currentNum == 1}">2022</button>
  <p>{ voting[currentNum].name }</p>

  <div style="display: flex; font-size: 12px; margin-bottom: 4px;">
    <div style="width: {voting[currentNum].approve.length * 100 /193}%;}">Approve</div>
    <div style="width: {voting[currentNum].abstain.length * 100 /193}%;}">Abstain</div>
    <div style="width: {voting[currentNum].absent.length * 100 /193}%;}">Absent</div>
    <div style="width: {voting[currentNum].against.length * 100 /193}%;}">Against</div>
  </div>
  <div style="display: flex; margin-bottom: 2rem;">
    <div class="bar-item" style="width: {voting[currentNum].approve.length * 100 /193}%; background: {color_approve};">{voting[currentNum].approve.length}</div>
    <div class="bar-item" style="width: {voting[currentNum].abstain.length * 100 /193}%; background: {color_abstain}">{voting[currentNum].abstain.length}</div>
    <div class="bar-item" style="width: {voting[currentNum].absent.length * 100 /193}%; background: {color_absent}">{voting[currentNum].absent.length}</div>
    <div class="bar-item" style="width: {voting[currentNum].against.length * 100 /193}%; background: {color_against}">{voting[currentNum].against.length}</div>
  </div>
</div>

<svg viewBox="0 0 1000 430" style="width: 100%; height: 100%;">
  <g>
    {#each features as feature, i}
      <path
        d={path(feature)}
        class="border"
        fill={selectColor(feature.properties.name, currentNum)}
        on:click={() => {selected = feature, console.log(selected.id, feature.id)}} 
      />
    {/each}
  </g>
  {#if selected}
		<path d={path(selected)} fill="hsl(0 0% 50% / 20%)" />
	{/if}
  <!-- <circle
    class="pin"
    cx={projection([30.5236, 50.45])[0]}
    cy={projection([30.5236, 50.45])[1]}
    r={2}
    fill={'red'} /> -->
  <!-- {#each countryList as pin}
    <circle
      class="pin"
      cx={projection(pin.location)[0]}
      cy={projection(pin.location)[1]}
      r={2}
      fill={'darkturquoise'}
      fill-opacity={0.6}
    />
  {/each} -->
  <!-- <path class="path-line" d={vueltos}></path> -->
</svg>

<div class="selectedName">{selected?.properties.name ?? ''}</div>