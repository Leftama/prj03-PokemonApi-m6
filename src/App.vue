<template>
  <div id="app">
    <img src="../public/imgs/pokemon-logo.png" alt="Logo Pokémon" class="pokemon-logo" />
    <h1>Adivina el Pokémon</h1>
    <p>Pokémones descubiertos: {{ discoveredCount }}/20</p>
    <div class="pokemon-container">
      <PokemonCard
        v-for="pokemon in pokemons"
        :key="pokemon.name"
        :name="pokemon.name"
        :imageUrl="pokemon.imageUrl"
        :onDiscover="increaseCounter"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';

export default {
  name: 'App',
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemons: [],
      discoveredCount: 0,
    };
  },
  methods: {
    fetchPokemons() {
      axios.get('https://pokeapi.co/api/v2/pokemon?limit=20')
        .then(response => {
          const pokemonPromises = response.data.results.map(pokemon => {
            return axios.get(pokemon.url).then(pokemonData => ({
              name: pokemonData.data.name,
              imageUrl: pokemonData.data.sprites.front_default,
            }));
          });
          Promise.all(pokemonPromises).then(pokemonList => {
            this.pokemons = pokemonList;
          });
        });
    },
    increaseCounter() {
      this.discoveredCount++;
    },
  },
  created() {
    this.fetchPokemons();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 50px;
  background-color: #f8f094; /* Fondo amarillo suave */
  padding: 20px; /* Espaciado alrededor del contenido */
  min-height: 100vh; /* Asegura que cubra toda la altura de la ventana */
}

.pokemon-logo {
  max-width: calc(100% - 700px);
  height: auto; /* Mantiene la proporción */
  margin-bottom: 20px; /* Espaciado entre el logo y el título */
}

h1 {
  color: #3b4cca; /* Color del título */
}

.pokemon-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
