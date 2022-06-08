
<script>
  import { geoAlbers, geoNaturalEarth1, geoConicEqualArea, geoPath } from "d3-geo";
  import { onMount } from "svelte";
  import { feature } from "topojson";
  let selected;
  let features = [];
  let paises = [];

  const projection = geoNaturalEarth1();
  const path = geoPath().projection(projection);

  onMount(async () => {
    const response = await fetch(
    "https://cdn.jsdelivr.net/npm/world-atlas@2/countries-50m.json"
    ).then(d => d.json())
    features = feature(response, response.objects.countries).features;
  });
</script>

<style>
  .border {
    stroke: #ddd;
  }
  @media (prefers-color-scheme: dark) {
    .border {
      stroke: #041a28;
    }
  }
</style>

<div class="selectedName">{selected?.properties.name ?? ''}</div>

<svg viewBox="0 0 1000 430" style="width: 100%; height: 100%;">
  <g>
    {#each features as feature, i}
      <path d={path(feature)} class="border" on:click={() => {selected = feature, console.log(selected.id, feature.id)}} fill="rgba(255,255,255,0.1)" />
    {/each}
  </g>
  {#if selected}
		<path d={path(selected)} fill="hsl(0 0% 50% / 20%)" />
	{/if}
</svg>
