<template>
  <div class="indicador-turno">
    <p :class="{ rojo: turnoActual === 1, verde: turnoActual === 2 }">
      Turno del Jugador {{ turnoActual }}
    </p>
    <button @click="mostrarReglas = true">Mostrar Reglas</button>
    
    <div v-if="mostrarReglas" class="mensaje-reglas">
      <div class="contenido-mensaje">
        <h2>Reglas del Juego</h2>
        <p>1. El objetivo del juego es llegar al lado opuesto del tablero.</p>
        <p>2. Los jugadores se mueven una casilla a la vez, en horizontal o vertical.</p>
        <p>3. Los jugadores pueden bloquear el camino de su oponente colocando bloqueos.</p>
        <p>4. Un jugador no puede moverse a una casilla ocupada por otro jugador.</p>
        <p>5. Los jugadores se mueven con las teclas A, S , D, W. </p>
        <p>6. Los Bloqueos se mueven con las Flechas del Teclado y se colocan con el Enter.</p>
        <p>7. Presionando el Espacio se cambia la Orientacion del Bloqueo.</p>
        <p>8. El bloqueo ubica apartir del ultimo que se haya puesto.</p>
        <button @click="mostrarReglas = false">Cerrar</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    turnoActual: Number
  },
  data() {
    return {
      mostrarReglas: false
    };
  }
};
</script>

<style scoped>
.indicador-turno {
  font-size: 18px;
  font-weight: bold;
  text-align: center;
}

.mensaje-reglas {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  border: 1px solid black;
  padding: 20px;
  z-index: 1000;
}

.contenido-mensaje {
  text-align: left;
}

.mensaje-reglas button {
  margin-top: 10px;
}
.rojo {
  color: red;
}

.verde {
  color: green;
}

</style>
