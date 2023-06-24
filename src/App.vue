<script setup>
  import { computed, ref } from 'vue';
  import { productos } from './data';
  import oferta from './assets/images/oferta.jpg';
  
  const contador = ref( 0 );
  const ruta = 'images/reyes/rey_';

  const siguiente = () => {
    contador.value++;
    if( contador.value === productos.length )
      contador.value = 0;
  }

  const rey = computed( () => {
    const firstLetter = productos[contador.value].nombre.charAt(0).toUpperCase();
    const rest = productos[contador.value].nombre.toLowerCase().slice(1);
    return firstLetter + rest;
  } )

  const imagen = computed( () => {
    const url = `${ruta}${productos[contador.value].nombre.toLowerCase()}.png`;
    return url;
  } )

  const nuevoPrecio = computed( () => {
    const precio = Number(productos[contador.value].precio*0.9).toFixed(2);
    return precio;
  } )

</script>

<template>

  <div class="container">

    <h2>
      Cena {{ contador + 1 }}
      con el rey godo <span class="resaltar">{{ rey }} </span>   
    </h2>
    <h3 class="precio">
      Precio: <span class="resaltar">${{ productos[contador].precio }}</span>
    </h3>
    <div v-if="productos[contador].finDeSemana" class="dias bg-red">(Solo fines de semana)</div>
    <div v-else class="dias bg-green">(De lunes a domingo)</div>
    <div v-if="productos[contador].precio<100" class="oferta">
      <div>
        Ahora un 10% dto:
        <span class="resaltar">${{ nuevoPrecio }}</span>
        <img :src="oferta" alt="imagen oferta">
      </div>      
    </div>
    <div>
      <img 
        :src="imagen" 
        :alt="`imagen del rey ${ rey }`" 
        class="imagen-rey"
      >
    </div>
    

    <button @click="siguiente">
      Siguiente ({{ contador + 1 }} / {{ productos.length }})
    </button>
  </div>

  
</template>

<style scoped>

  .container{
    border: 1px solid #ccc;
    padding: 20px 10px;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  h2{
    text-align: center;
  }

  .precio{
    text-align: center;
    margin: 15px;  
  }

  .resaltar{
    color: green;
    font-weight: bold;
    font-size: 1.1em;
  }

  .dias{
    color: #fff;
    padding: 4px 17px;
    font-size: 0.9em;
    border-radius: 5px;
    margin: 0 0 10px;
    display: inline-block;
  }

  .bg-red{
    background-color: red;
  }

  .bg-green{
    background-color: green;
  }

  .oferta img{
    width: 65px;
    margin: 12px 5px;
  }

  .oferta div{
    display: flex;
    align-items: center;
  }

  .imagen-rey{
    width: 250px;
  }

  button{
    margin: 20px 0 0;
    font-weight: bold;
    padding: 5px;
  }

</style>
