<template>
    <div v-if="mostrar" id="mensaje-ganador">
      {{ mensaje }}
    </div>
  </template>
  
  <script>
  export default {
    props: {
      mostrar: Boolean,
      mensaje: String,
    },
  };
  </script>
  
  <style scoped>
  #mensaje-ganador {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 20px;
    background-color: white;
    border: 2px solid black;
    font-size: 24px;
    text-align: center;
  }
  </style>
  