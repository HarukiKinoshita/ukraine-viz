<script>
  import { LayerCake, Svg, Canvas } from 'layercake';
	import { scaleTime } from 'd3-scale';
	import { timeParse } from 'd3-time-format';
	
  import AxisTime from './AxisTime.svelte';
  import Scatter from './Scatter.svelte';
  import Line from './Line.svelte';
  import DataPoint from './DataPoint.svelte';

  import data from './data';

  data.forEach(d => {
    d.date = timeParse("%Y-%m-%d")(d.date);
		d.date = new Date(d.date);
    console.log(d.date);
  })
</script>

<div class="chart-container">
  <LayerCake
    padding={{ right: 10, bottom: 20, left: 25 }}
    xPadding={[5, 5]}
    xScale={scaleTime()}
    xNice={true}
    yPadding={[0, 10]}
    yDomain={[0, null]}
    yNice={true}
    x={(d) => d.date}
    y={(d) => d.group}
    {data}
  >
    <!-- x={(d) => d[0]}
    y={(d) => d[1]} -->
    <Svg>
      <AxisTime />
    </Svg>

    <Canvas>
      <Line fill="#0cf" />
			<Scatter r={4.5} fill="#ff0" />
    </Canvas>
  </LayerCake>
</div>

<style>
  .chart-container {
    /* width: 100vw; */
    padding: 5vw;
    height: 40vh;
  }
</style>
