<template>
  <div
    :class="['bloqueo', direccion, jugadorClase]"
    :style="{ top: (fila * 44) + 'px', left: (columna * 44) + 'px', width: (direccion === 'horizontal' ? '80px' : '10px'), height: (direccion === 'vertical' ? '80px' : '10px') }"
    @click="manejarClic"
  ></div>
</template>

<script>
export default {
  props: {
    fila: Number,
    columna: Number,
    direccion: String, 
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
  background-color: gray;
  cursor: pointer;
}
.jugador1 {
  background-color: blue;
}
.jugador2 {
  background-color: red;
}
</style>
