<script>
  import * as d3 from "d3"
  import { onMount } from "svelte";

  let datos = []

  let aplicaciones = d3
    .scaleOrdinal()
    .domain(["Facebook", "Tiktok", "Instagram", "Twitter", "Otros"])
    .range(["#14ff00", "#ff0000", "#cc00ff", "#0500ff", "#ffd600"])

  let margen_left = d3.scaleLinear()
    .domain([0, 5])
    .range([0, 100])

  let margen_bottom = d3.scaleLinear()
    .domain([0, 5])
    .range([0, 100])

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
  <div class="quadrant">
    <div class="ejemplo-alumno">

    </div>
  </div>
  <div class="quadrant">

  </div>
  <div class="quadrant">

  </div>
  <div class="quadrant">

  </div>
</div>



</main>

<style>

.grilla {
    display: grid;
    grid-template-columns: 1fr 1fr;
    height: 100vh; 
  }
  .quadrant {
    width: 45vw;
    height: 50vh;
    border: 1px solid #000;
    position: relative;
  }
  .ejemplo-alumno{
    width: 50px;
    height: 50px;
    background-color: red;
    position: absolute;
    top:50%;
    left: 30%;
  }
</style>
