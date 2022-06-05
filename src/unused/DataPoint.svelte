<script>
  import { getContext } from 'svelte';

  const { data, xGet, yGet, width, height } = getContext('LayerCake');
</script>

<g class="datapoints">
  {#each data as d}
    <circle
      cx={xGet(d)}
      cy={yGet(d)}
      r=5
    />
  {/each}
</g>