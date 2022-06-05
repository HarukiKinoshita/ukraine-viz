<script>
  import { getContext } from 'svelte';
  import { scaleCanvas } from 'layercake';

  const { data, xGet, yGet, width, height } = getContext('LayerCake');

  const { ctx } = getContext('canvas');

  export let r;
  // export let fill;

  $: {
    if ($ctx) {
			// NOTE: Comment next two lines with moving to one <Cavnas />
      //scaleCanvas($ctx, $width, $height);
      //$ctx.clearRect(0, 0, $width, $height);

      $data.forEach((d) => {
          $ctx.beginPath();
          $ctx.arc($xGet(d), $yGet(d), r, 0, 2 * Math.PI, false);
          //$ctx.fillStyle = fill;
          $ctx.fill();
          //$ctx.strokeStyle = "#000";
          $ctx.stroke();
        })
    }
  }
</script>
