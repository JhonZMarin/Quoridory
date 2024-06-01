<template>
  <div id="app">
    <div class="content">
      <IndicadorTurno :turnoActual="turnoActual" />
      <div class="info-bloqueos">
        <p>Jugador 1 Bloqueos Disponibles: {{ 10 - cantidadBloquesJugador('jugador1') }}</p>
        <p>Jugador 2 Bloqueos Disponibles: {{ 10 - cantidadBloquesJugador('jugador2') }}</p>
      </div>
      <Tablero 
        :casillas="casillas" 
        :bloqueos="bloqueos" 
        @moverJugador="moverJugador" 
        @colocarBloqueo="colocarBloqueo" 
      />
      <MensajeGanador :mostrar="mostrarGanador" :mensaje="mensajeGanador" />
      <div class="botones-bloqueo">
        <button @click="() => activarModoBloqueo('jugador1')" :disabled="modoBloqueo || turnoActual !== 1">Bloquear J1</button>
        <button @click="() => activarModoBloqueo('jugador2')" :disabled="modoBloqueo || turnoActual !== 2">Bloquear J2</button>
      </div>
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
      turnoActual: 1,
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
      casillas[0 * 9 + 4].jugadorClase = 'jugador1';
      casillas[8 * 9 + 4].jugadorClase = 'jugador2';
      return casillas;
    },
    cantidadBloquesJugador(jugador) {
      return this.bloqueos.filter(bloqueo => bloqueo.jugadorClase === jugador).length;
    },
    actualizarTurno() {
      this.turnoActual = this.turnoActual === 1 ? 2 : 1;
    },
    moverJugador(fila, columna) {
      const jugador = this.turnoActual === 1 ? 'jugador1' : 'jugador2';
      const posicionActual = this.posiciones[jugador];
      const casillaDestino = this.casillas[fila * 9 + columna];

      if (!casillaDestino.jugadorClase && this.esMovimientoValido(posicionActual, { fila, columna })) {
        this.casillas[posicionActual.fila * 9 + posicionActual.columna].jugadorClase = '';
        this.casillas[fila * 9 + columna].jugadorClase = jugador;
        this.posiciones[jugador] = { fila, columna };

        if (this.verificarGanador(fila)) {
          this.mostrarGanador = true;
          this.mensajeGanador = `¡Jugador ${this.turnoActual} ha ganado!`;
        } else {
          this.actualizarTurno();
        }
      }
    },
    esMovimientoValido(posActual, nuevaPos) {
      const distFila = Math.abs(nuevaPos.fila - posActual.fila);
      const distColumna = Math.abs(nuevaPos.columna - posActual.columna);
      const esMovimientoBasicoValido = (distFila === 1 && distColumna === 0) || (distFila === 0 && distColumna === 1);

      if (!esMovimientoBasicoValido) return false;

      for (const bloqueo of this.bloqueos) {
        if (bloqueo.direccion === 'horizontal') {
          if ((posActual.fila === bloqueo.fila && nuevaPos.fila === bloqueo.fila + 1) || 
              (posActual.fila === bloqueo.fila + 1 && nuevaPos.fila === bloqueo.fila)) {
            if (posActual.columna === bloqueo.columna || posActual.columna === bloqueo.columna + 1) {
              return false;
            }
          }
        } else if (bloqueo.direccion === 'vertical') {
          if ((posActual.columna === bloqueo.columna && nuevaPos.columna === bloqueo.columna + 1) || 
              (posActual.columna === bloqueo.columna + 1 && nuevaPos.columna === bloqueo.columna)) {
            if (posActual.fila === bloqueo.fila || posActual.fila === bloqueo.fila + 1) {
              return false;
            }
          }
        }
      }

      return true;
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
      }
      else {
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
 
      const jugadorActual = this.turnoActual === 1 ? 'jugador1' : 'jugador2';
      const cantidadBloqueosDisponibles = this.bloqueos.filter(bloqueo => bloqueo.jugadorClase === jugadorActual).length;
      if (cantidadBloqueosDisponibles >= 10) {
        
        return;
      }

      if (direccion === 'horizontal' && fila < 8) {
        this.bloqueos.push({ fila, columna, direccion, jugadorClase: jugadorActual });
      } else if (direccion === 'vertical' && columna < 8) {
        this.bloqueos.push({ fila, columna, direccion, jugadorClase: jugadorActual });
      }
      this.modoBloqueo = false;
      this.actualizarTurno();
    },
    activarModoBloqueo(jugador) {

      const cantidadBloqueosDisponibles = this.bloqueos.filter(bloqueo => bloqueo.jugadorClase === jugador).length;
      if (cantidadBloqueosDisponibles >= 10) {

        return;
      }

      this.modoBloqueo = true;
    }
  },
  mounted() {
    window.addEventListener('keydown', this.manejarTecla);
  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.manejarTecla);
  },
};
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  background-color: #f0f0f0;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.info-bloqueos {
  margin-bottom: 10px;
}

.botones-bloqueo {
  display: flex;
  justify-content: space-around;
  width: 100%;
  max-width: 400px;
  margin-top: 20px;
}
</style>
