<template>
    <div class="pokemon-card">
      <img :src="imageUrl" :class="{ hidden: !isDiscovered }" alt="Pokémon silhouette" />
      <div v-if="!isDiscovered">
        <input ref="guessInput" v-model="guess" @keyup.enter="checkGuess" placeholder="Nombre del Pokémon"/>
        <button @click="checkGuess">Descubrir</button>
      </div>
      <div v-else>
        <p>{{ name }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      name: String,
      imageUrl: String,
      onDiscover: Function,
    },
    data() {
      return {
        guess: '',
        isDiscovered: false,
      };
    },
    methods: {
        checkGuess() {
            if (this.guess.toLowerCase() === this.name.toLowerCase()) {
                this.isDiscovered = true;
                this.onDiscover();
            } else {
                alert('El nombre ingresado es incorrecto');
                this.resetInput();
            }
        },
        resetInput() {
            this.guess = '';
            this.$refs.guessInput.focus();
        },
    },
  };
  </script>
  
  <style scoped>
  .pokemon-card {
    border: 1px solid #ccc;
    border-radius: 10px; /* Redondear esquinas */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Sombra suave */
    padding: 20px; /* Espaciado interior */
    text-align: center;
    margin: 10px;
    background-color: white; /* Fondo blanco para las tarjetas */
  }
  
  .hidden {
    filter: blur(10px);
  }
  
  input {
    display: block;
    width: 80%;
    margin: 10px auto; /* Centrar el input y agregar margen */
  }
  
  button {
    padding: 5px 10px;
    border: 2px solid;
    background-color: #ffcb05; /* Color amarillo */
    color: #3b4cca; /* Color de texto */
    cursor: pointer;
    border-radius: 5px;
    transition: background-color 0.3s;
    display: block;
    width: 50%;
    margin: 0 auto; /* Centrar el botón */
  }
  
  button:hover {
    border-color: #000000; /* Cambiar color del borde al pasar el ratón por encima */
    background-color: #E71B1B; /* Color al pasar el ratón por encima */
    color: #FFFFFF;
  }
  </style>
  