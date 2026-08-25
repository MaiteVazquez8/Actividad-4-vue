<script setup>
import { computed, ref } from 'vue'
import eeveeImagen from './assets/eevee.webp'
import vaporeonImagen from './assets/vaporeon.png'
import jolteonImagen from './assets/jolteon.png'
import espeonImagen from './assets/espeon.webp'
import glaceonImagen from './assets/glaceon.webp'
import sylveonImagen from './assets/Sylveon.webp'
import flareonImagen from './assets/flareon.webp'
import umbreonImagen from './assets/umbreon.png'
import leafeonImagen from './assets/Leafeon.webp'

const esGameOver = ref(false)
const esBrillante = ref(false)
const evolucionSeleccionada = ref('eevee')

const evoluciones = [
  { id: 'eevee', nombre: 'Eevee', tipo: 'Normal', color: '#9b7653', descripcion: 'Un Pokemon adaptable que puede evolucionar de muchas formas.', imagen: eeveeImagen },
  { id: 'vaporeon', nombre: 'Vaporeon', tipo: 'Agua', color: '#4b9fe1', descripcion: 'Puede disolverse en el agua y vivir en mares y rios.', imagen: vaporeonImagen },
  { id: 'jolteon', nombre: 'Jolteon', tipo: 'Electrico', color: '#f4c542', descripcion: 'Acumula electricidad en su pelaje y la lanza a gran velocidad.', imagen: jolteonImagen },
  { id: 'espeon', nombre: 'Espeon', tipo: 'Psiquico', color: '#c875b8', descripcion: 'Predice los movimientos de sus rivales con sus poderes psiquicos.', imagen: espeonImagen },
  { id: 'glaceon', nombre: 'Glaceon', tipo: 'Hielo', color: '#8ed8e8', descripcion: 'Puede congelar el aire y crear cristales de hielo.', imagen: glaceonImagen },
  { id: 'sylveon', nombre: 'Sylveon', tipo: 'Hada', color: '#e987b5', descripcion: 'Emite un aura tranquilizadora con sus cintas y lazos.', imagen: sylveonImagen },
  { id: 'flareon', nombre: 'Flareon', tipo: 'Fuego', color: '#ed7441', descripcion: 'Guarda aire caliente en su saco interno para lanzar fuego.', imagen: flareonImagen },
  { id: 'umbreon', nombre: 'Umbreon', tipo: 'Siniestro', color: '#665b8f', descripcion: 'Sus anillos brillan en la oscuridad cuando sale a cazar.', imagen: umbreonImagen },
  { id: 'leafeon', nombre: 'Leafeon', tipo: 'Planta', color: '#6caf62', descripcion: 'Realiza la fotosintesis y desprende un aroma fresco.', imagen: leafeonImagen },
]

const evolucionActual = computed(() => evoluciones.find(({ id }) => id === evolucionSeleccionada.value))
</script>

<template>
  <div class="pantalla-juego" :class="{ 'game-over-screen': esGameOver }">
    
    <div class="caja-items">
      <h2><span class="bloque-interrogacion">●</span> Pokédex de Eevee</h2>
      
      <div class="grupo-botones">
        <p>Pokemon elegido:</p>
        <button class="btn-eevee seleccionado">Eevee</button>
      </div>

      <div class="grupo-botones">
        <p>Elegir evolución por tipo:</p>
        <button v-for="evolucion in evoluciones.slice(1)" :key="evolucion.id" :style="{ '--color-tipo': evolucion.color }" :class="{ activo: evolucion.id === evolucionSeleccionada }" @click="evolucionSeleccionada = evolucion.id">
          {{ evolucion.tipo }}
        </button>
      </div>

      <div class="grupo-botones checkbox-container">
        <label>
          <input type="checkbox" v-model="esGameOver" /> 💀 Activar GAME OVER
        </label>
        <label>
          <input type="checkbox" v-model="esBrillante" /> ✨ Activar brillo
        </label>
      </div>
    </div>

    <div class="escenario">
      
     
      <div class="interfaz-personaje">

<div class="marcador">
  <span class="texto-retro">{{ evolucionActual.nombre.toUpperCase() }}</span>
  <span class="texto-retro">TIPO: {{ evolucionActual.tipo.toUpperCase() }}</span>
</div>

    <div class="contenedor-sprite" :class="{ 'estado-gameover': esGameOver, 'estado-brillante': esBrillante }">
      <img :src="evolucionActual.imagen" :alt="evolucionActual.nombre" class="sprite" />
      <div v-if="esGameOver" class="cartel-gameover">GAME OVER</div>

</div>

        <div class="inventario-visual">
          <p>Información de la evolución:</p>
          <span class="etiqueta-item">{{ evolucionActual.descripcion }}</span>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped>

.pantalla-juego {
  display: flex;
  flex-wrap: wrap;
  gap: 30px;
  padding: 30px;
  background: linear-gradient(135deg, #f7f1d5 0%, #d8efc5 48%, #8bcf9b 100%);
  font-family: 'Courier New', Courier, monospace;
  min-height: 450px;
  border-radius: 15px;
  transition: all 0.5s ease;
}


.game-over-screen {
  background-color: #000000 !important;
}

.caja-items {
  background: rgba(255, 255, 255, 0.9);
  padding: 20px;
  border-radius: 10px;
  width: 320px;
  border: 4px solid #d9292f;
}

.bloque-interrogacion {
  background: #d9292f;
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

.grupo-botones button {
  background: var(--color-tipo, #f8b800);
  color: #111;
}

.grupo-botones button.activo {
  outline: 3px solid #111;
  outline-offset: 1px;
}

.seleccionado {
  cursor: default;
  box-shadow: 0 0 0 3px #111;
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

.estado-gameover {
  filter: grayscale(1);
}

.estado-brillante {
  filter: drop-shadow(0 0 12px #fff) drop-shadow(0 0 26px #ffe66d) brightness(1.2);
  animation: brillo 1.2s ease-in-out infinite alternate;
}

.estado-gameover.estado-brillante {
  filter: grayscale(1) drop-shadow(0 0 12px #fff) brightness(1.2);
}

.sprite {
  width: 80px;
  height: 80px;
}


.cartel-gameover {
  color: #e52521;
  text-align: center;
  font-size: 24px;
  font-weight: bold;
  text-shadow: 2px 2px #000;
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


@keyframes parpadeo {
  0%, 100% { background: #d9292f; }
  50% { background: #9e1d25; }
}

@keyframes brillo {
  from { filter: drop-shadow(0 0 8px #fff) drop-shadow(0 0 14px #ffe66d) brightness(1.05); }
  to { filter: drop-shadow(0 0 18px #fff) drop-shadow(0 0 32px #ffe66d) brightness(1.3); }
}

@media (max-width: 760px) {
  .pantalla-juego {
    padding: 16px;
  }

  .caja-items {
    width: 100%;
  }

  .escenario {
    min-height: 360px;
  }
}
</style>

