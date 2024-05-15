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

  function gradiente(alumno){
    let colors = color_aplicacion(alumno);
    if (colors.length > 1) {
        return `linear-gradient(${colors.join(",")})`;
    } else {
        return colors[0];
    }
  }

  let genero = d3
    .scaleOrdinal()
    .domain(["Femenino", "Masculino", "Otro"])
    .range(["./images/fem.svg", "./images/masc.svg", "./images/otro.svg",]);

  let tiempo = d3
    .scaleQuantize()
    .domain([1,5])
    .range(["./images/1hora.svg", "./images/2horas.svg", "./images/3horas.svg", "./images/4horas.svg", "./images/5horas.svg"]);

  let posicion_horizontal = d3
    .scaleOrdinal()
    .domain(["No", "Si"])
    .range(["top: 0px", "bottom: 0px"]);
  
  let posicion_vertical = d3
    .scaleOrdinal()
    .domain(["No", "Si"])
    .range(["left: 100%", "left: 0px"]);

  /* 

  Armo una función que me retorna 
  la ruta si yo le paso el índice

  */

  function getImagePath(alumno) {
    if (alumno.horas_celular == 2){  
      return "./images/circulo.svg";
    }
    if (alumno.horas_celular == 3){
      return "./images/triang.svg";
    }
    if (alumno.horas_celular == 4){
      return "./images/cuadrado.svg";
    }
    if (alumno.horas_celular == 5){
      return "./images/penta.svg";
    }
    if (alumno.horas_celular == 6){
      return "./images/hexa.svg";
    }
  }
  function obtenerDatosAlumno(alumno) {
    let datos = `Tiempo de ocio: ${alumno.tiempo_ocio}\n`;
    datos += `Tiempo de trabajo: ${alumno.tiempo_trabajo}\n`;
    datos += `Aplicaciones:\n`;

    if (alumno.tiktok == 1) {
      datos += `- TikTok\n`;
    }
    if (alumno.twitter == 1) {
      datos += `- Twitter\n`;
    }
    if (alumno.instagram == 1) {
      datos += `- Instagram\n`;
    }
    if (alumno.facebook == 1) {
      datos += `- Facebook\n`;
    }
    if (alumno.otros == 1) {
      datos += `- Otros\n`;
    }

    return datos;
  }

  onMount(() => {
    d3.csv("./datos_redes.csv", d3.autoType).then((data) => {
      datos = data;
      console.log(datos);
      datos.forEach(alumno => {
        color_aplicacion(alumno);
        getImagePath(alumno);
        gradiente(alumno);
        obtenerDatosAlumno(alumno);

      });
    });
  });
</script>

<main>
  <head>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Jersey+25&display=swap" rel="stylesheet">

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Archivo+Black&family=Lilita+One&family=Ubuntu+Mono:ital,wght@0,400;0,700;1,400;1,700&display=swap" rel="stylesheet">
  </head>
  
  <h1>Habitos Digitales</h1>
  <h2 style="text-align:center; margin: 7px">¿Cuánto usamos nuestro celular? ¿Para que lo usamos? ¿Cómo nos hace sentir?</h2>
  <p style="text-align:center">Vivimos en un mundo rodeado de tecnología, donde el celular se ha convertido en una extensión de nuestra vida cotidiana. Este proyecto te invita a descubrir de un vistazo cuáles son tus hábitos digitales: qué tanto tiempo que dedicás a tu teléfono, cuáles son tus aplicaciones favoritas, qué impacto emocional tiene en vos...</p>

  <div class="container">
    {#each datos as alumno}
      <div class="alumno-general">
        <div class="borde">
          <div class="info">
            <div class="horas-genero" >
              <div class="figura">
                <img src="{getImagePath(alumno)}" alt=""style="background: {gradiente(alumno)}" title="{obtenerDatosAlumno(alumno)}">
              </div>
            </div>
          </div>
          <div class="borde-horizontal" style="{posicion_horizontal(alumno.perjudica)}; ">
            <img src="{tiempo(alumno.tiempo_ocio)}" alt="" style="width: 100%">
          </div>
          <div class="borde-vertical" style="{posicion_vertical(alumno.considera_tiempo)};">
            <img src="{tiempo(alumno.tiempo_trabajo)}" alt="" style="width: 100%">
          </div>
          <div class="genero">
            <img src="{genero(alumno.genero)}" alt="">
          </div>      
        </div>

        <div class="usuario">
          <p>{alumno.nombre}</p>
        </div>

      </div>
    {/each}
    
    

    <div class="explicacion">
      <div class="columna">
        <h2>Tiempo promedio de uso del celular</h2>
        <div class="fotos">
          <div class="foto-container">
            <img src="./images/circulo.svg" style= "background-color: #000000" alt="">
            <p>1-2 horas</p>
          </div>
          <div class="foto-container">
            <img src="./images/triang.svg" style="background-color: #000000" alt="">
            <p>2-3 horas</p>
          </div>
          <div class="foto-container">
            <img src="./images/cuadrado.svg" style="background-color: #000000" alt="">
            <p>3-4 horas</p>
          </div>
          <div class="foto-container">
            <img src="./images/penta.svg" style="background-color: #000000" alt="">
            <p>4-5 horas</p>
          </div>
          <div class="foto-container">
            <img src="./images/hexa.svg" style="background-color: #000000" alt="">
            <p>+5 horas</p>
          </div>
        </div>
      </div>
    
      <div class="columna">
        <h2>Genero</h2>
        <div class="fotos">
          <div class="foto-container">
            <img src="./images/femi.svg" alt="">
            <p>Femenino</p>
          </div>
          <div class="foto-container">
            <img src="./images/mascu.svg" alt="">
            <p>Masculino</p>
          </div>
          <div class="foto-container">
            <img src="./images/otrx.svg" alt="">
            <p>Otro</p>
          </div>
        </div>
      </div>
    </div>
    <div class="explicacion">
      <div class="columna">
        <h2>Colores</h2>
        <div class="fotos">
          <div class="foto-container">
            <div class="caja morado"></div>
            <p>Instagram</p>
          </div>
          <div class="foto-container">
            <div class="caja azul"></div>
            <p>Twitter</p>
          </div>
          <div class="foto-container">
            <div class="caja rojo"></div>
            <p>Tik Tok</p>
          </div>
          <div class="foto-container">
            <div class="caja verde"></div>
            <p>Facebook</p>
          </div>
          <div class="foto-container">
            <div class="caja amarillo"></div>
            <p>Otros</p>
          </div>
        </div>
      </div>
    </div>
    <div class="explicacion">  
      <div class="columna">
        <h2>La grilla</h2>
        <div>
        <p>La cantidad de lineas representa la cantidad de horas dedicada para el ocio y para el trabajo</p>
        <div class="fotos">
          <div class="foto-container">
            <img src="./images/1hora.svg" alt="">
            <p>1 hora</p>
          </div>
          <div class="foto-container">
            <img src="./images/2horas.svg" alt="">
            <p>2 horas</p>
          </div>
          <div class="foto-container">
            <img src="./images/3horas.svg" alt="">
            <p>3 horas</p>
          </div>
          <div class="foto-container">
            <img src="./images/4horas.svg" alt="">
            <p>4 horas</p>
          </div>
          <div class="foto-container">
            <img src="./images/5horas.svg" alt="">
            <p>5 horas</p>
          </div>
        </div>
      
	    <div class="columna">
          <p>La posicion de estas lineas representan la posicion dentro de la grilla</p>
          <div class="fotos">
            <div class="foto-container">
              <img src="./images/sisi.svg" alt="">
              <p>Considera que utiliza mucho el celular y lo perjudica</p>
            </div>
            <div class="foto-container">
              <img src="./images/sino.svg" alt="">
              <p>Considera que utiliza mucho el celular y no lo perjudica</p>
            </div>
            <div class="foto-container">
              <img src="./images/nosi.svg" alt="">
              <p>No considera que utiliza mucho el celular y lo perjudica</p>
            </div>
            <div class="foto-container">
              <img src="./images/nono.svg" alt="">
              <p>No considera que utiliza mucho el celular y no lo perjudica</p>
            </div>
          </div>
        </div>
      </div>

    </div>
</main>

<style>
  main{
    margin: 0;
    padding: 0;
    background-color: white;
  }
  h1{
    font-family: "Jersey 25", sans-serif;
    font-weight: 400;
    font-style: normal;
    text-align: center;
    margin: 20px;
  }
  h2{
    font-family: "Ubuntu Mono", sans-serif;
    font-optical-sizing: auto;
    font-weight: 600;
    font-style: normal;
  }
  p{
    font-family: "Ubuntu Mono", sans-serif;
    font-optical-sizing: auto;
    font-weight: 400;
    font-style: normal;
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
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
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
    width: 100%; 
    bottom: 0;
    transform-origin: left bottom; 
    transform: rotate(-90deg); 
  }
  .info{
    width: 100%;
    margin: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .borde-horizontal img{
    width: auto;
    height: 100%;
    display: block;
    
  }
  .borde-vertical img{
    width: auto;
    height: 100%;
    display: block;
    
  }
  .explicacion{
    display: flex;
    justify-content: center;
    align-items: flex-start;
    gap: 10px;
    flex-direction: row;
    justify-content: center;
    width:1000px;
  }
  .columna {
    flex: 1; 
    padding: 0 10px; 
  }
  .fotos {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    flex-wrap: wrap;
    gap: 20px;
  }
  .foto-container {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    gap: 3px;
  }
  .caja {
      width: 120px;
      height: 20px; 
      border-radius: 5px; 
    }

    .rojo { background-color: #ff0000; }
    .azul { background-color: #0500ff;}
    .verde { background-color: #14ff00; }
    .amarillo { background-color: #ffd600; }
    .morado { background-color: #cc00ff; }
</style>
