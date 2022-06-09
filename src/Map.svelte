
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

  $: color_approve = "#14B8A6"
  $: color_against = "#F43F5E"
  $: color_abstain = "#EAB308"
  $: color_absent = "#64748B"

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
  }
  @media (prefers-color-scheme: dark) {
    .border {
      stroke: #041a28;
      stroke-width: 0.5;
    }
  }
  .voting td {
    padding: 4px 6px;
    min-width: 10vw;
  }
</style>

<div>
  <button on:click={() => {currentNum = 0}}>Crimia</button>
  <button on:click={() => {currentNum = 1}}>Ukraine</button>
  <h2>{ voting[currentNum].name }</h2>
  <table class="voting" align="center">
    <tr>
      <td style="background: {color_approve}">Approved</td>
      <td style="background: {color_against}">Against</td>
      <td style="background: {color_abstain}">Abstain</td>
      <td style="background: {color_absent}">Absent</td>
    </tr>
    <tr style="font-size: 3rem; font-weight: bold;">
      <td>{ voting[currentNum].approve.length }</td>
      <td>{ voting[currentNum].against.length }</td>
      <td>{ voting[currentNum].abstain.length }</td>
      <td>{ voting[currentNum].absent.length }</td>
    </tr>
  </table>
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