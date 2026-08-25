<script setup>
import { ref, computed } from 'vue'

const personajeSeleccionado = ref('mario')
const esGrande = ref(false)
const tieneFuego = ref(false)
const monedas = ref(0)
const esGameOver = ref(false)
const mundoFuego = ref(false)

const inventario = computed(() => {
  if (personajeSeleccionado.value === 'mario') {
    return ['Gorra clásica M', 'Overol azul', 'Guantes blancos']
  } else if (personajeSeleccionado.value === 'luigi') {
    return ['Gorra verde L', 'Overol azul oscuro']
  } else if (personajeSeleccionado.value === 'toad') {
    return ['Chaleco azul y oro', 'Sombrero de hongo', 'Pantalón blanco']
  }
  return []
})

const ganarMoneda = () => {
  monedas.value++
}

const cambiarPersonaje = (nombre) => {
  personajeSeleccionado.value = nombre
  esGrande.value = false
  tieneFuego.value = false
}

</script>

<template>
  <div class="pantalla-juego" :class="{ 'mundo-fuego': mundoFuego, 'game-over-screen': esGameOver }">
    
    <div class="caja-items">
      <h2><span class="bloque-interrogacion">?</span> Caja de Ítems</h2>
      
      <div class="grupo-botones">
        <p>Elegir Personaje:</p>
        <button @click="cambiarPersonaje('mario')" class="btn-mario">Mario</button>
        <button @click="cambiarPersonaje('luigi')" class="btn-luigi">Luigi</button>
        <button @click="cambiarPersonaje('toad')" class="btn-toad">Toad</button>
      </div>

      <div class="grupo-botones flex-botones">
  <p>Poderes:</p>
  
  <button @click="esGrande = !esGrande" class="btn-poder">
    <img src="@/assets/Champi.webp" alt="Champiñón" class="icono-poder" />
    Champiñón (Grande/Chico)
  </button>
  
  <button @click="tieneFuego = !tieneFuego" :disabled="!esGrande" class="btn-poder">
    <img src="@/assets/Flor_de_fuego.webp" alt="Flor de fuego" class="icono-poder" />
    Flor de Fuego
  </button>
</div>


      <div class="grupo-botones">
        <p>Acciones:</p>
        <button @click="ganarMoneda" class="btn-moneda"> Moneda (+1)</button>
        <button @click="mundoFuego = !mundoFuego"> Cambiar de Mundo</button>
      </div>

      <div class="grupo-botones checkbox-container">
        <label>
          <input type="checkbox" v-model="esGameOver" /> 💀 Activar GAME OVER
        </label>
      </div>
    </div>

    <div class="escenario">
      
     
      <div v-if="esGameOver" class="cartel-gameover">
        <h1>GAME OVER</h1>
        <p>Presiona el botón para continuar</p>
      </div>
      

      <div v-else class="interfaz-personaje">

<div class="marcador">
  <span class="texto-retro">{{ personajeSeleccionado.toUpperCase() }}</span>
  <span class="texto-retro contador-monedas">
    <img src="@/assets/Monedas.webp" alt="Moneda" class="icono-moneda" />
    x {{ monedas }}
  </span>
</div>

    <div class="contenedor-sprite" :class="{ 'sprite-grande': esGrande, 'estado-fuego': tieneFuego }">
  

  <img v-if="personajeSeleccionado === 'mario'" src="@/assets/Mario.png" alt="Mario" class="sprite" />
  

  <img v-else-if="personajeSeleccionado === 'luigi'" src="@/assets/Luigi.webp" alt="Luigi" class="sprite" />
  

  <img v-else src="@/assets/Toad.png" alt="Toad" class="sprite" />

</div>

        <div class="inventario-visual">
          <p>Equipamiento activo:</p>
          <span v-for="(item, index) in inventario" :key="index" class="etiqueta-item">
            ⭐ {{ item }}
          </span>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped>

.pantalla-juego {
  display: flex;
  gap: 30px;
  padding: 30px;
  background-color: #5c94fc; 
  font-family: 'Courier New', Courier, monospace;
  min-height: 450px;
  border-radius: 15px;
  transition: all 0.5s ease;
}


.mundo-fuego {
  background-color: #e64a19 !important;
}
.game-over-screen {
  background-color: #000000 !important;
}

.caja-items {
  background: rgba(255, 255, 255, 0.9);
  padding: 20px;
  border-radius: 10px;
  width: 320px;
  border: 4px solid #f8b800;
}

.bloque-interrogacion {
  background: #f8b800;
  color: white;
  padding: 2px 8px;
  border-radius: 4px;
  animation: parpadeo 1s infinite;
}

.grupo-botones {
  margin-bottom: 15px;
  border-bottom: 1px dashed #ccc;
  padding-bottom: 10px;
}

.grupo-botones p {
  margin: 0 0 5px 0;
  font-weight: bold;
}

button {
  padding: 8px 12px;
  margin: 4px;
  cursor: pointer;
  font-weight: bold;
  border: 2px solid #000;
  border-radius: 4px;
}

.btn-mario { background: #e52521; color: white; }
.btn-luigi { background: #00b130; color: white; }
.btn-toad { background: #ffffff; color: #e52521; }
.btn-moneda { background: #fcc200; }


.escenario {
  flex-grow: 1;
  background: rgba(0, 0, 0, 0.2);
  border: 4px dashed white;
  border-radius: 10px;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 20px;
}

.marcador {
  display: flex;
  justify-content: space-between;
  color: white;
  font-size: 20px;
  font-weight: bold;
  text-shadow: 2px 2px #000;
}

.contenedor-sprite {
  text-align: center;
  margin: auto;
  transition: transform 0.3s ease, filter 0.3s ease;
}

.sprite {
  width: 80px;
  height: 80px;
}


.sprite-grande {
  transform: scale(1.6); 
}

.estado-fuego {
  filter: drop-shadow(0px 0px 15px #ff5722) sepia(0.5) hue-rotate(340deg);
}

.cartel-gameover {
  color: #e52521;
  text-align: center;
  margin: auto;
}

.cartel-gameover h1 {
  font-size: 45px;
  margin: 0;
  letter-spacing: 5px;
}

.inventario-visual {
  color: white;
  font-size: 12px;
  text-shadow: 1px 1px #000;
}

.etiqueta-item {
  background: rgba(0, 0, 0, 0.5);
  padding: 3px 8px;
  border-radius: 10px;
  margin-right: 5px;
}


.contador-monedas {
  display: flex;
  align-items: center;
  gap: 8px; 
}

.icono-moneda {
  width: 24px;   
  height: 24px;  
  object-fit: contain;
}


.btn-poder {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 8px 12px;
}

.icono-poder {
  width: 22px;   
  height: 22px;   
  object-fit: contain;
}

@keyframes parpadeo {
  0%, 100% { background: #f8b800; }
  50% { background: #b87800; }
}
</style>

