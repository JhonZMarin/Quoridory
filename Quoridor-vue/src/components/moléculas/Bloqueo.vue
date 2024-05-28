<template>
  <div 
    :class="['bloqueo', direccion, jugadorClase]"
    @click="manejarClic"
    tabindex="0"
  ></div>
</template>

<script>
export default {
  props: {
    fila: Number,
    columna: Number,
    direccion: String, // 'horizontal' o 'vertical'
    jugadorClase: String
  },
  methods: {
    manejarClic() {
      this.$emit('click', this.fila, this.columna, this.direccion);
    }
  }
};
</script>

<style scoped>
.bloqueo {
  position: absolute;
  width: 40px; /* Ajustar según el tamaño del tablero */
  height: 10px; /* Ajustar según el tamaño del tablero */
  background-color: gray;
  cursor: pointer;
}
.horizontal {
  width: 80px;
  height: 10px;
}
.vertical {
  width: 10px;
  height: 80px;
}
.jugador1 {
  background-color: blue;
}
.jugador2 {
  background-color: red;
}
</style>
