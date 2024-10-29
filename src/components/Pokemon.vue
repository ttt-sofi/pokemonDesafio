<template>
  <div class="pokemon-instance">
    <h2>¿Quién es este Pokémon?</h2>

    <!-- Imagen del Pokémon con filtros aplicados inicialmente -->
    <img
      :src="pokemon.imageUrl"
      :alt="pokemon.name"
      :class="{ filtered: applyFilter }"
    />

    <br />

    <!-- Campo de entrada y botón de verificación -->
    <input
      type="text"
      v-model="pokemon.guess"
      placeholder="Escribe el nombre"
      @input="resetFilters"
      class="input"
      @keyup.enter="checkGuess"
    />
    <button v-if="showButton" @click="checkGuess" class="button">
      Verificar
    </button>

    <!-- Mensajes de éxito o error -->
    <p v-if="pokemon.correct" class="success">
      Es <strong>{{ pokemon.name }}!</strong>
    </p>
    <p v-if="pokemon.incorrect" class="error">Vuelve a intentarlo</p>
  </div>
</template>

<script>
export default {
  name: "Pokemon",
  props: {
    pokemon: Object, // Objeto con datos del Pokémon
    applyFilter: Boolean, // Estado de filtro inicial de la imagen
    showButton: Boolean, // Estado de visibilidad del botón
  },
  methods: {
    checkGuess() {
      const userGuess = this.pokemon.guess.toLowerCase();
      if (userGuess === this.pokemon.name) {
        this.pokemon.correct = true;
        this.pokemon.incorrect = false;
        this.$emit("updateFilter", false); // Emitir eventos para modificar estado
        this.$emit("updateButton", false);
      } else {
        this.pokemon.incorrect = true;
      }
    },
    resetFilters() {
      this.$emit("updateFilter", true);
      this.$emit("updateButton", true);
      this.pokemon.correct = false;
      this.pokemon.incorrect = false;
    },
  },
};
</script>

<style scoped>
.pokemon-instance {
  margin: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  text-align: center;
  background-color: #f8f8f8;
}

img.filtered {
  filter: grayscale(100%) blur(5px);
  transition: filter 0.5s ease;
}

img {
  width: auto;
  height: 150px;
}

.error {
  color: red;
}

.success {
  color: green;
}

input[type="text"] {
  padding: 10px;
  width: 80%;
  margin: 10px 0;
  font-size: 16px;
  border: 2px solid #ddd;
  border-radius: 5px;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s ease;
}

input[type="text"]:focus {
  border-color: #3b82f6; /* Color azul cuando se hace foco */
  outline: none;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  color: #fff;
  background-color: #3b82f6; /* Color azul */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease, transform 0.2s ease;
}

button:hover {
  background-color: #2563eb; /* Azul más oscuro al pasar el mouse */
  transform: scale(1.05); /* Ligeramente más grande */
}

button:active {
  transform: scale(0.98); /* Ligero efecto de "clic" */
}
</style>
