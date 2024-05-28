<template>
  <div id="tablero" class="tablero">
    <!-- Renderizar las casillas del tablero -->
    <div 
      v-for="(casilla, index) in casillas"
      :key="index"
      :class="['casilla', casilla.jugadorClase]"
      @click="manejadorClic(casilla.fila, casilla.columna)"
    ></div>

    <!-- Renderizar los bloques en el tablero -->
    <Bloqueo
      v-for="(bloqueo, index) in bloqueos"
      :key="index"
      :fila="bloqueo.fila"
      :columna="bloqueo.columna"
      :direccion="bloqueo.direccion"
      :jugadorClase="bloqueo.jugadorClase"
    />
  </div>
</template>

<script>
import Bloqueo from '../moléculas/Bloqueo.vue';

export default {
  components: {
    Bloqueo,
  },
  props: {
    casillas: Array,
    bloqueos: Array
  },
  methods: {
    manejadorClic(fila, columna) {
      this.$emit('moverJugador', fila, columna);
    }
  }
};
</script>

<style scoped>
.tablero {
  display: grid;
  grid-template-columns: repeat(9, 1fr);
  grid-template-rows: repeat(9, 1fr);
  width: 400px;
  height: 400px;
  border: 1px solid black;
  position: relative;
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
