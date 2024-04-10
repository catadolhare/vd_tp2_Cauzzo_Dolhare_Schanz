<script>
  import * as d3 from "d3"
  import { onMount } from "svelte";

  let datos = []

  onMount(() => {
    d3.csv('./datos_redes.csv', d3.autoType)
      .then(data => {
        datos = data
        console.log(datos)
    })
  })


</script>

<main>

<div class="grilla">

</div>






</main>

<style>
  
</style>
