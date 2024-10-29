<template>
  <div>
    <h1>Adivina el Pokémon</h1>
    <div id="app" class="pokemon-container">
      <Pokemon
        v-for="(pokemon, index) in pokemons"
        :key="pokemon.name"
        :pokemon="pokemon"
        :applyFilter="applyFilter[index]"
        :showButton="showButton[index]"
        @updateFilter="updateFilter(index, $event)"
        @updateButton="updateButton(index, $event)"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon.vue";

export default {
  name: "App",
  components: {
    Pokemon,
  },
  data() {
    return {
      pokemons: [],
      applyFilter: [],
      showButton: [],
    };
  },
  methods: {
    async fetchPokemons() {
      try {
        const response = await axios.get(
          "https://pokeapi.co/api/v2/pokemon?limit=20&offset=0"
        );
        this.pokemons = await Promise.all(
          response.data.results.map(async (pokemonData) => {
            const pokemonDetails = await axios.get(pokemonData.url);
            return {
              name: pokemonDetails.data.name,
              imageUrl: pokemonDetails.data.sprites.front_default,
              guess: "",
              correct: false,
              incorrect: false,
            };
          })
        );

        this.applyFilter = Array(this.pokemons.length).fill(true);
        this.showButton = Array(this.pokemons.length).fill(true);
      } catch (error) {
        console.error("Error fetching Pokémon:", error);
      }
    },
    updateFilter(index, value) {
      this.applyFilter[index] = value;
    },
    updateButton(index, value) {
      this.showButton[index] = value;
    },
  },
  mounted() {
    this.fetchPokemons();
  },
};
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  display: grid;
  grid-template-columns: repeat(4, 1fr); /* 4 columnas */
  gap: 10px;
}

h1 {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
}
</style>
