<template>
  <div class="tablero">
    <div 
      v-for="casilla in casillas" 
      :key="`${casilla.fila}-${casilla.columna}`" 
      :class="['casilla', casilla.jugadorClase]"
      @click="manejarClickCasilla(casilla.fila, casilla.columna)">
    </div>
    <div 
      v-for="bloqueo in bloqueos" 
      :key="`${bloqueo.fila}-${bloqueo.columna}-${bloqueo.direccion}`" 
      :class="['bloqueo', bloqueo.direccion]"
      :style="obtenerEstiloBloqueo(bloqueo)">
    </div>
  </div>
</template>

<script>
export default {
  props: {
    casillas: Array,
    bloqueos: Array
  },
  methods: {
    manejarClickCasilla(fila, columna) {
      this.$emit('moverJugador', fila, columna);
    },
    obtenerEstiloBloqueo(bloqueo) {
      if (bloqueo.direccion === 'horizontal') {
        return {
          top: `${bloqueo.fila * 50 + 45}px`,
          left: `${bloqueo.columna * 50}px`,
          width: '100px',
          height: '5px',
          backgroundColor: 'blue'
        };
      } else if (bloqueo.direccion === 'vertical') {
        return {
          top: `${bloqueo.fila * 50}px`,
          left: `${bloqueo.columna * 50 + 45}px`,
          width: '5px',
          height: '100px',
          backgroundColor: 'blue'
        };
      }
    }
  }
}
</script>

<style>
.tablero {
  position: relative;
  width: 450px;
  height: 450px;
  display: grid;
  grid-template-columns: repeat(9, 50px);
  grid-template-rows: repeat(9, 50px);
}

.casilla {
  width: 50px;
  height: 50px;
  border: 1px solid black;
}

.jugador1 {
  background-color: red;
}

.jugador2 {
  background-color: green;
}

.bloqueo {
  position: absolute;
}

.bloqueo.horizontal {
  height: 5px;
  background-color: blue;
}

.bloqueo.vertical {
  width: 5px;
  background-color: blue;
}
</style>
