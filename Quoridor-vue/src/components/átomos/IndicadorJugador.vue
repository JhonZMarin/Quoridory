<template>
  <span :id="`jugador${numeroJugador}-turno`">
    Jugador {{ numeroJugador }}: <span v-if="esTurno">&#128293;</span>
  </span>
</template>

<script>
export default {
  props: {
    numeroJugador: Number,
    esTurno: Boolean,
  },
};
</script>
