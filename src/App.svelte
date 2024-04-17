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
    let altura= d3.scaleLinear()
      .domain([0, 5])
      .range([0, 5])
  })
  
</script>

<main>

<div class="grilla">
  <div class="quadrant">Quadrant 1</div>
  <div class="quadrant">Quadrant 2</div>
  <div class="quadrant">Quadrant 3</div>
  <div class="quadrant">Quadrant 4</div>

</div>


</main>

<style>

.grilla {
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    height: 100vh; 
  }
  .quadrant {
    width: 45vw;
    height: 50vh;
    border: 1px solid #000;
    box-sizing: border-box;
    justify-items: center; 
    align-items: center; 
    position: relative;
  }
</style>
