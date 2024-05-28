<template>
  <div id="indicador-turno">
    <IndicadorJugador :numeroJugador="1" :esTurno="turnoActual === 1" />
    <br />
    <IndicadorJugador :numeroJugador="2" :esTurno="turnoActual === 2" />
  </div>
</template>

<script>
import IndicadorJugador from '../átomos/IndicadorJugador.vue';

export default {
  components: {
    IndicadorJugador,
  },
  props: {
    turnoActual: {
      type: Number,
      required: true
    }
  }
};
</script>
