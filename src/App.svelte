<script>
  import * as d3 from "d3"
  import { onMount } from "svelte";

  let datos = []

  let aplicaciones = d3
    .scaleOrdinal()
    .domain(["Facebook", "Tiktok", "Instagram", "Twitter", "Otros"])
    .range(["#14ff00", "#ff0000", "#cc00ff", "#0500ff", "#ffd600"])

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

<div class="container-principal">

  <div class="grilla">
  <div class="quadrant">Quadrant 1</div>
  <div class="quadrant">Quadrant 2</div>
  <div class="quadrant">Quadrant 3</div>
  <div class="quadrant">Quadrant 4</div>
  </div>

</div>

</main>

<style>

.grilla {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: repeat(2, 1fr);
    height: 100vh; 
  }
  .quadrant {
    width: 45vw;
    height: 50vh;
    border: 1px solid #000;
    box-sizing: border-box;
    display: grid; 
    grid-template-columns: repeat(5, 1fr); 
    grid-template-rows: repeat(5, 1fr);
    justify-items: center; 
    align-items: center; 
    position: relative;
  }
</style>
