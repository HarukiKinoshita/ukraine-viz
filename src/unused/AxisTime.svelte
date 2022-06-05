<script>
  import { getContext } from 'svelte';

  const { data, xScale, yRange, height, width } = getContext('LayerCake');

  // label margin
  export let xTick = 0;
  export let yTick = 15;
  export let dxTick = 0;
  export let dyTick = 0;

  $: formatTime = $xScale.tickFormat()
  $: tickVals = $xScale.ticks(5);
</script>

<g class="axis x-axis">
  {#each tickVals as tick, i}
    <g class="tick tick-{i}" transform="translate({$xScale(tick)},{$yRange[0]})">
      <!-- Gridline -->
      <line class="gridline" y1={$height * -1} y2="0" x1="0" x2="0" />

      <!-- Tick marks -->
      <!-- <line class="tick-mark" y1="0" y2="6" x1={xTick} x2={xTick} /> -->

      <!-- Label -->
      <text x={xTick} y={yTick} dx={dxTick} dy={dyTick} text-anchor=middle>
        {formatTime(tick)}
      </text>
    </g>
  {/each}
  <line class="baseline" y1={$height} y2={$height} x1={0} x2={$width} />
</g>

<style>
  .tick {
    font-size: 0.725em;
    font-weight: 200;
  }

  .tick text {
    fill: #666;
  }

  line {
    stroke: #aaa;
    stroke-dasharray: 2;
  }

  .baseline {
    stroke-dasharray: 0;
  }
</style>
