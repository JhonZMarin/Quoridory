<template>
  <div 
    id="tablero" 
    class="tablero"
    @mousemove="actualizarPosicionBloqueo"
  >
    <!-- Renderizar las casillas del tablero -->
    <div 
      v-for="(casilla, index) in casillas"
      :key="index"
      :class="['casilla', casilla.jugadorClase]"
      @click="manejadorClic(casilla.fila, casilla.columna)"
    ></div>

    <!-- Resaltar la posición del bloque al pasar el mouse sobre el tablero -->
    <Bloqueo
      v-if="posicionBloqueo"
      :fila="posicionBloqueo.fila"
      :columna="posicionBloqueo.columna"
      :direccion="direccionBloqueo"
      :jugadorClase="`jugador${turnoActual}`"
      @click="colocarBloqueo"
    />
  </div>
</template>

<script>
import Bloqueo from '../moléculas/Bloqueo.vue';

export default {
  props: {
    casillas: Array,
    turnoActual: Number
  },
  data() {
    return {
      posicionBloqueo: null,
      direccionBloqueo: 'horizontal' // Dirección predeterminada del bloqueo
    };
  },
  methods: {
    manejadorClic(fila, columna) {
      this.$emit('moverJugador', fila, columna);
    },
    actualizarPosicionBloqueo(event) {
      // Calcular la fila y columna del bloque según la posición del mouse
      const rect = event.target.getBoundingClientRect();
      const offsetX = event.clientX - rect.left;
      const offsetY = event.clientY - rect.top;
      const fila = Math.floor(offsetY / 44); // Calcular fila
      const columna = Math.floor(offsetX / 44); // Calcular columna

      // Actualizar la posición del bloque
      this.posicionBloqueo = { fila, columna };
    },
    colocarBloqueo(fila, columna, direccion) {
      // Emitir un evento con la posición y dirección del bloqueo
      this.$emit('colocarBloqueo', fila, columna, direccion);
    }
  },
  components: {
    Bloqueo
  }
};
</script>

<style scoped>
.tablero {
  position: relative;
  display: grid;
  grid-template-columns: repeat(9, 1fr);
  grid-template-rows: repeat(9, 1fr);
  width: 400px;
  height: 400px;
  border: 1px solid black;
}
.casilla {
  border: 1px solid black;
}
.jugador1 {
  background-color: rgb(116, 178, 230);
}
.jugador2 {
  background-color: rgb(226, 44, 83);
}
</style>
