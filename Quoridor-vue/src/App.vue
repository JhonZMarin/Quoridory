<template>
  <div id="app">
    <IndicadorTurno :turnoActual="turnoActual" />
    <Tablero :casillas="casillas" :bloqueos="bloqueos" @moverJugador="moverJugador" @colocarBloqueo="colocarBloqueo" />
    <MensajeGanador :mostrar="mostrarGanador" :mensaje="mensajeGanador" />
    <div class="botones-bloqueo">
      <button @click="activarModoBloqueo" :disabled="modoBloqueo || turnoActual !== 1">Bloquear J1</button>
      <button @click="activarModoBloqueo" :disabled="modoBloqueo || turnoActual !== 2">Bloquear J2</button>
    </div>
  </div>
</template>

<script>
import IndicadorTurno from './components/organismos/IndicadorTurno.vue';
import Tablero from './components/moléculas/Tablero.vue';
import MensajeGanador from './components/átomos/MensajeGanador.vue';

export default {
  components: {
    IndicadorTurno,
    Tablero,
    MensajeGanador,
  },
  data() {
    return {
      turnoActual: 1, // 1 para Jugador 1, 2 para Jugador 2
      mostrarGanador: false,
      mensajeGanador: '',
      casillas: this.crearCasillas(),
      posiciones: {
        jugador1: { fila: 0, columna: 4 },
        jugador2: { fila: 8, columna: 4 }
      },
      bloqueos: [],
      modoBloqueo: false,
      direccionBloqueo: 'horizontal',
      posicionBloqueo: { fila: 0, columna: 0 }
    };
  },
  methods: {
    crearCasillas() {
      const casillas = [];
      for (let i = 0; i < 9; i++) {
        for (let j = 0; j < 9; j++) {
          casillas.push({ fila: i, columna: j, jugadorClase: '' });
        }
      }
      // Posiciones iniciales de los jugadores
      casillas[0 * 9 + 4].jugadorClase = 'jugador1'; // Centro superior
      casillas[8 * 9 + 4].jugadorClase = 'jugador2'; // Centro inferior
      return casillas;
    },
    actualizarTurno() {
      this.turnoActual = this.turnoActual === 1 ? 2 : 1;
    },
    moverJugador(fila, columna) {
      const jugador = this.turnoActual === 1 ? 'jugador1' : 'jugador2';
      const posicionActual = this.posiciones[jugador];

      if (this.esMovimientoValido(posicionActual, { fila, columna })) {
        // Actualizar casillas
        this.casillas[posicionActual.fila * 9 + posicionActual.columna].jugadorClase = '';
        this.casillas[fila * 9 + columna].jugadorClase = jugador;

        // Actualizar posición del jugador
        this.posiciones[jugador] = { fila, columna };

        // Verificar si hay ganador
        if (this.verificarGanador(fila)) {
          this.mostrarGanador = true;
          this.mensajeGanador = `¡Jugador ${this.turnoActual} ha ganado!`;
        } else {
          // Actualizar el turno
          this.actualizarTurno();
        }
      }
    },
    esMovimientoValido(posActual, nuevaPos) {
      const distFila = Math.abs(nuevaPos.fila - posActual.fila);
      const distColumna = Math.abs(nuevaPos.columna - posActual.columna);
      return (distFila === 1 && distColumna === 0) || (distFila === 0 && distColumna === 1);
    },
    verificarGanador(fila) {
      return (this.turnoActual === 1 && fila === 8) || (this.turnoActual === 2 && fila === 0);
    },
    manejarTecla(event) {
  const jugador = this.turnoActual === 1 ? 'jugador1' : 'jugador2';
  const posicionActual = this.posiciones[jugador];
  let nuevaFila = posicionActual.fila;
  let nuevaColumna = posicionActual.columna;

  if (this.modoBloqueo) {
    if (event.key === 'ArrowUp' && this.posicionBloqueo.fila > 0) this.posicionBloqueo.fila--;
    if (event.key === 'ArrowDown' && this.posicionBloqueo.fila < 8) this.posicionBloqueo.fila++;
    if (event.key === 'ArrowLeft' && this.posicionBloqueo.columna > 0) this.posicionBloqueo.columna--;
    if (event.key === 'ArrowRight' && this.posicionBloqueo.columna < 8) this.posicionBloqueo.columna++;
  } else {
    if (event.key === 'w' && nuevaFila > 0) nuevaFila--;
    if (event.key === 's' && nuevaFila < 8) nuevaFila++;
    if (event.key === 'a' && nuevaColumna > 0) nuevaColumna--;
    if (event.key === 'd' && nuevaColumna < 8) nuevaColumna++;
    this.moverJugador(nuevaFila, nuevaColumna);
  }

  if (event.key === ' ') {
    this.direccionBloqueo = this.direccionBloqueo === 'horizontal' ? 'vertical' : 'horizontal';
  }

  if (event.key === 'Enter' && this.modoBloqueo) {
    this.colocarBloqueo(this.posicionBloqueo.fila, this.posicionBloqueo.columna, this.direccionBloqueo);
  }
},
    colocarBloqueo(fila, columna, direccion) {
      const jugador = this.turnoActual === 1 ? 'jugador1' : 'jugador2';
      if (this.modoBloqueo) {
        this.bloqueos.push({ fila, columna, direccion, jugadorClase: jugador });
        this.modoBloqueo = false;
        this.actualizarTurno();
      }
    },
    activarModoBloqueo() {
      this.modoBloqueo = true;
    }
  },
  mounted() {
    window.addEventListener('keydown', this.manejarTecla);
  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.manejarTecla);
  }
};
</script>

<style>
.botones-bloqueo {
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
}
</style>
