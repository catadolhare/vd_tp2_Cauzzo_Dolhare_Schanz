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
  <head>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Jersey+25&display=swap" rel="stylesheet">
  </head>
  
  <h1>Uso del celular y redes sociales</h1>

  <div class="grilla">

    <div class="quadrant">
      {#each datos as alumno}
        {#if alumno.considera_tiempo == "No" && alumno.perjudica == "Si"}
          <div class="alumno-general" style="bottom: {margen_bottom(alumno.tiempo_trabajo)}%; left: {margen_left(alumno.tiempo_redes)}%">
            <img src={getImagePath(horas_celular(alumno.horas_celular))} alt="" />
          </div>
        {/if}          
      {/each}
    </div>

    <!--En este cuadrante no se le agrega el style-->
    <div class="quadrant">
      {#each datos as alumno}
        {#if alumno.considera_tiempo == "Si" && alumno.perjudica == "Si"}
          <div class="alumno-general" style="bottom: {margen_bottom(alumno.tiempo_trabajo)}%: left: {margen_left(alumno.tiempo_redes)}%">
            <img src={getImagePath(horas_celular(alumno.horas_celular))} alt="" />
          </div>
        {/if}          
      {/each}
    </div>

    <!--En este cuadrante los que estan más arriba se pasan al cuadrante de arriba-->
    <div class="quadrant">
      {#each datos as alumno}
        {#if alumno.considera_tiempo == "Si" && alumno.perjudica == "No"}
          <div class="alumno-general" style="bottom: {margen_bottom(alumno.tiempo_trabajo)}%; left: {margen_left(alumno.tiempo_redes)}%">
            <img src={getImagePath(horas_celular(alumno.horas_celular))} alt="" />
          </div>
        {/if}          
      {/each}
    </div>

    <div class="quadrant">
      {#each datos as alumno}
        {#if alumno.considera_tiempo == "No" && alumno.perjudica == "No"}
          <div class="alumno-general" style="bottom: {margen_bottom(alumno.tiempo_trabajo)}%; left: {margen_left(alumno.tiempo_redes)}%">
            <img src={getImagePath(horas_celular(alumno.horas_celular))} alt="" />
          </div>
        {/if}          
      {/each}
    </div>
  </div>

  <div class="explicacion">
    <div class="todos-simbolos">
      <h2>Símbolos</h2>
      <p>Los simbolos representan la cantidad de horas utilizando el celular</p>
      <div class="cont-simbolos">
        <div class="simbolo">
          <img src="./images/circulo.svg" alt="" class="color-figure"  />
          <p>2 horas</p>
        </div>
        <div class="simbolo">
          <img src="./images/triangulo.svg" alt="" class="color-figure" />
          <p>3 horas</p>
        </div>
        <div class="simbolo">
          <img src="./images/cuadrado.svg" alt="" class="color-figure" />
          <p>4 horas</p>
        </div>
        <div class="simbolo">
          <img src="./images/pentagono.svg" alt="" class="color-figure" />
          <p>5 horas</p>
        </div>
        <div class="simbolo">
          <img src="./images/hexagono.svg" alt="" class="color-figure" />
          <p>6 horas</p>
        </div>
      </div>
    </div>
    <div class="aplicaciones">
      <h2>Aplicaciones</h2>
      <div class="cont-apps-color">
        <div class="cont-apps">
          <div class="app"><p>Instagram</p></div>
            <div class="color" style="background-color: {aplicaciones("Instagram")}"></div>
        </div>
        <div class="cont-apps">
          <div class="app"><p>Facebook</p></div>
            <div class="color" style="background-color: {aplicaciones("Facebook")}"></div>
        </div>
        <div class="cont-apps">
          <div class="app"><p>Tiktok</p></div>
            <div class="color" style="background-color: {aplicaciones("Tiktok")}"></div>
        </div>
        <div class="cont-apps">
          <div class="app"><p>Twitter</p></div>
            <div class="color" style="background-color: {aplicaciones("Twitter")}"></div>
        </div>
        <div class="cont-apps">
          <div class="app"><p>Otros</p></div>
            <div class="color" style="background-color: {aplicaciones("Otros")}"></div>
        </div>
      </div>
    </div>
  </div>
</main>

<style>
  .grilla {
    display: grid;
    grid-template-columns: 1fr 1fr;
    height: 100vh;
    background-color: white;
  }
  .quadrant {
    width: 45vw;
    height: 50vh;
    border: 1px solid #000;
    position: relative;
  }
  .alumno-general {
    width: 40px;
    height: 40px;
    background-color: red;
    position: absolute;
  }
  .alumno-general img {
    width: 100%;
    height: 100%;
  }
  h1{
    font-family: "Jersey 25", sans-serif;
    font-weight: 400;
    font-style: normal;
    text-align: center;
  }
  .explicacion{
    display: flex;
    flex-wrap: wrap;
  }
  .todos-simbolos{
    width: 50%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .cont-simbolos{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }
  .simbolo{
    display: flex;
    flex-direction: column;
    margin-left: 3px;
  }
  .aplicaciones{
    width:50%;
    display: flex;
    flex-direction: column;

  }
  .cont-apps-color{
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    width: 200px;
  }
  .cont-apps{
    width: 100%;
    display: flex;
    flex-direction: row;
    
  }
  .color{
    width: 25%;
    margin-left: 50px;
  }
  .app{
    width: 50%;
  }
  .color-figure {
    filter: brightness(0) invert(1) sepia(1) saturate(1000%) hue-rotate(150deg) grayscale(100%);
  }
</style>
