<script>
  import { getContext } from 'svelte';
  import { scaleCanvas } from 'layercake';

  const { data, xGet, yGet, width, height } = getContext('LayerCake');

  const { ctx } = getContext('canvas');

  export let fill;

  $: {
    if ($ctx) {
      scaleCanvas($ctx, $width, $height);
      $ctx.clearRect(0, 0, $width, $height);

      $ctx.beginPath();
      $data.forEach((d) => {
          $ctx.lineTo($xGet(d), $yGet(d));
      });
      $ctx.strokeStyle = fill;
      $ctx.stroke();

    }
  }
</script>
