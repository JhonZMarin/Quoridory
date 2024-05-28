<template>
  <div :class="['casilla', jugadorClase]" @click="manejadorClic"></div>
</template>

<script>
export default {
  props: {
    fila: Number,
    columna: Number,
    jugadorClase: String
  },
  methods: {
    manejadorClic() {
      this.$emit('click');
    }
  }
};
</script>

<style scoped>
.casilla {
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid black;
}
</style>
