<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";

  let datos = [];
  //Armo un array con las rutas de los SVG
  let rutasImagenes = [
      "./images/circulo.svg",
      "./images/triangulo.svg",
      "./images/cuadrado.svg",
      "./images/pentagono.svg",
      "./images/hexagono.svg",
    ];

  let aplicaciones = d3
    .scaleOrdinal()
    .domain(["Facebook", "Tiktok", "Instagram", "Twitter", "Otros"])
    .range(["#14ff00", "#ff0000", "#cc00ff", "#0500ff", "#ffd600"]);

  let margen_left = d3.scaleLinear().domain([0, 5]).range([0, 100]);

  let margen_bottom = d3.scaleLinear().domain([0, 5]).range([0, 100]);

  let horas_celular = d3
    .scaleLinear()
    .domain([2, 3, 4, 5, 6])
    .range([0, 1, 2, 3, 4]); // Valores numéricos que representan el índice de cada elemento de rutasImagenes

  /* 

  Armo una función que me retorna 
  la ruta si yo le paso el índice

  */
  
  function getImagePath(indexImage) {
    return rutasImagenes[indexImage];
  }

  onMount(() => {
    d3.csv("./datos_redes.csv", d3.autoType).then((data) => {
      datos = data;
      console.log(datos);
    });
  });
</script>

<main>
  <div class="grilla">
    <div class="quadrant">
        <!-- Busca la ruta de la imagen a partir 
        del numero que le retorna horas_celular, 
        que es el índice del elemento en rutasImagenes -->
      {#each datos as alumno}
        {#if alumno.considera_tiempo == "si" && alumno.perjudica == "si"}
          <div class="alumno-general" style=" bottom: {margen_bottom(alumno.tiempo_trabajo)}, left: {margen_left(alumno.tiempo_redes)}">
            <img src={getImagePath(horas_celular(alumno.horas_celular))} alt="" />
          </div>
        {/if}          
      {/each}
    </div>
    <div class="quadrant"></div>
    <div class="quadrant"></div>
    <div class="quadrant"></div>
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
  .alumno-general {
    width: 100px;
    height: 100px;
    background-color: red;
    position: absolute;
  }
</style>
