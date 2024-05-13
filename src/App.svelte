<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";

  let datos = [];
  function color_aplicacion(alumno){
    let apps =[];
    if(alumno.tiktok==1){
      apps.push("#ff0000");
    }
    if(alumno.twitter==1){
      apps.push("#0500ff");
    }
    if(alumno.instagram==1){
      apps.push("#cc00ff");
    }
    if(alumno.facebook==1){
      apps.push("#14ff00");
    }
    if(alumno.otros==1){
      apps.push("#ffd600");
    }
    return apps;
  }
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

  let horas_celular = d3
    .scaleLinear()
    .domain([2, 3, 4, 5, 6])
    .range([0, 1, 2, 3, 4]); // Valores numéricos que representan el índice de cada elemento de rutasImagenes

  let genero = d3
    .scaleOrdinal()
    .domain(["Femenino", "Masculino", "Otro"])
    .range(["./images/fem.svg", "./images/masc.svg", "./images/otro.svg",]);

  let tiempo = d3
    .scaleQuantize()
    .domain([1,5])
    .range(["./images/1hora.svg", "./images/2horas.svg", "./images/3horas.svg", "./images/4horas.svg", "./images/5horas.svg"]);

  let posicion = d3
    .scaleBand()
    .domain(["Si", "No"])
    .range([0, 100]);

  /* 

  Armo una función que me retorna 
  la ruta si yo le paso el índice

  */
  
  function getImagePath(alumno) {
    if (alumno.horas_celular == 2){  
      return "./images/circulo.svg";
    }
    if (alumno.horas_celular == 3){
      return "./images/triangulo.svg";
    }
    if (alumno.horas_celular == 4){
      return "./images/cuadrado.svg";
    }
    if (alumno.horas_celular == 5){
      return "./images/pentagono.svg";
    }
    if (alumno.horas_celular == 6){
      return "./images/hexagono.svg";
    }
  }
  
  function generarSVG(alumno) {
    const imagePath = getImagePath(alumno);
    const colores = color_aplicacion(alumno);

    let svg = `<svg width="120" height="120">`;
    
    if (imagePath) {
      svg += `<image href="${imagePath}" width="120" height="120" style="fill: `;
      
      if (colores.length == 1) {
        svg += `${colores[0]};`;
      } 
      svg += `"/></svg>`;
    }

    return svg;
  }
    
    
  let svgData = {};
  onMount(() => {
    d3.csv("./datos_redes.csv", d3.autoType).then((data) => {
      datos = data;
      console.log(datos);
      datos.forEach(alumno => {
        color_aplicacion(alumno);
        getImagePath(alumno);
        generarSVG(alumno);
      });
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

  <div class="container">
    {#each datos as alumno}
      <div class="alumno-general">

        <div class="borde">

          <div class="info">
            <div class="horas-genero">
              <div class="figura">
                <img src="{getImagePath(alumno)}" alt="">
                <!--{@html generarSVG(alumno)}-->
              </div>
  
              <div class="genero">
                <img src="{genero(alumno.genero)}" alt="">
              </div>
            </div>
          </div>
          
          <div class="borde-horizontal" style="top:{posicion(alumno.perjudica)}; ">
            <img src="{tiempo(alumno.tiempo_ocio)}" alt="" class="borde-horizontal" style="top:{posicion(alumno.perjudica)};">
          </div>
          <div class="borde-vertical" style="left:{posicion(alumno.perjudica)};">
            <img src="{tiempo(alumno.tiempo_trabajo)}" alt="" style="">
          </div>

        </div>

        <div class="usuario">
          <p>{alumno.nombre}</p>
        </div>

      </div>
    {/each}
  </div>
</main>

<style>
  main{
    background-color: antiquewhite;
    margin: 0;
    padding: 0;
  }
  h1{
    font-family: "Jersey 25", sans-serif;
    font-weight: 400;
    font-style: normal;
    text-align: center;
  }
  .container {
    display: flex;
    justify-content: center;
    align-items: end;
    margin: auto;
    flex-wrap: wrap;
    max-width: 1000px;
    gap: 30px;
    margin-bottom: 100px;
  }
  .alumno-general {
    display: flex;
    flex-direction: column;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    flex: 150px 0 0;
  }
  .horas-genero{
    width: 100%;
    position: relative;
  }
  .figura{
    width: 100%;
    height: 100%;
  }
  .figura img{
    width: 100%;
    height: 100%;
  }
  .genero img {
    width: 100%;
    height: 100%;
  }
  .genero{
    width: 20px;
    height: 20px;
    position: absolute;
    top: 23%;
    left: 42%;
    font-size: 40px;
    color: black;
    font-weight: bold;
  }
  .borde{
    width:100%;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .borde-horizontal{
    position: absolute;
    width: 100%;
    height: 5px;
    padding: 0;
    margin: 0;
  }
  .borde-vertical{
    position: absolute;
    width: 100%; /* Hacemos que el div ocupe todo el ancho del contenedor */
    height: 5px; /* Cambiado a 5px para que sea más estrecho */
    bottom: 0;
    transform-origin: left bottom; /* Establecemos el origen de la rotación */
    transform: rotate(-90deg); /* Rotamos el div */
  }
  .info{
    width: 100%;
    margin: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .borde-vertical img{
    width: auto;
    height: 100%;
    display: block;
    
  }

</style>
