<template>
  <header class="header">
    <div class="logo">
      <img src="@/assets/pngwing.com.png" alt="Logo_pokemon" />
    </div>
    <p>BUSCA TU POKEMON</p>
    <label>NOMBRE, TIPO o ID:</label>
    <div class="searchcontainer">
      <input
        type="text"
        placeholder="Pikachu"
        class="pokemonSearch"
        v-model="pokemonID"
      />
      <button @click="searchPokemon">Search pokemon!</button>
    </div>
  </header>
  <main class="container">
    <section class="cards-pokemon" v-if="pokemons">
      <div v-for="(pokemon, i) in pokemons" :key="i">
        <PokemonCard :pokemon="pokemon" @removePokemon="removePokemon" />
      </div>
    </section>
  </main>
</template>

<script>
import { pokeapi } from "@/api/pokeapi";
import PokemonCard from "./components/PokemonCard";
export default {
  name: "app",
  components: { PokemonCard },
  data() {
    return {
      pokemonID: "",
      pokemons: [],
    };
  },
  methods: {
    async searchPokemon() {
      try {
        const pokemonToFind = await fetch(`${pokeapi}${this.pokemonID}`);
        const pokemon = await pokemonToFind.json(); // La respuesta se convierte en un json
        /* this.pokemons.push(pokemon); */
        this.addPokemon(pokemon);
        /* console.log(this.pokemon); */
        return pokemon;
      } catch (error) {
        alert("Pokemon was not found");
        console.log(error);
      }
    },

    addPokemon(pokemon) {
      this.pokemons.push(pokemon);
    },
    removePokemon(id) {
      const index = this.pokemons.findIndex((pokemon) => pokemon.id === id);
      this.pokemons.splice(index, 1);
    },
  },
  /*   removePost(id) {
    this.tasks = this.tasks.filter((task) => task.id !== id);
  }, */
};
</script>

<style lang="scss">
@import "./pokemon_types.scss";
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
html {
  font-size: 62.2%;
}

@font-face {
  font-family: "poke";
  src: url("./fonts/PokemonGb-RAeo.ttf");
}

.header {
  display: flex;
  flex-direction: column;
  width: auto;
  height: 300px;
  background-color: black;
  justify-content: space-around;
  align-items: center;
}
.logo img {
  width: auto;
  height: 150px;
}
.header label {
  font-family: "poke";
  color: red;
  margin: 5px;
}
.header p {
  font-family: "poke";
  color: red;
  font-size: 17px;
  margin-bottom: 5px;
}
.searchcontainer {
  display: flex;
  flex-direction: row;
  margin: 5px;
}
.pokemonSearch {
  background-color: #ffcb32;
  width: 200px;
  height: 40px;
  border: none;
  padding-top: 5px;
  text-align: center;
  color: black;
  font-family: "poke";
  font-size: 13px;
  -webkit-border-top-left-radius: 20px;
  -webkit-border-bottom-left-radius: 20px;
  -moz-border-radius-topleft: 20px;
  -moz-border-radius-bottomleft: 20px;
  border-top-left-radius: 20px;
  border-bottom-left-radius: 20px;
}
.pokemonSearch::placeholder {
  text-align: center;
  color: black;
  font-family: "poke";
}
.pokemonSearch:focus::placeholder {
  color: transparent;
}
button {
  width: 200px;
  height: 40px;
  background-color: #2d63c8;
  border: none;
  letter-spacing: 0.5px;
  cursor: pointer;
  font-size: 13px;
  font-family: "poke";
  -webkit-border-top-right-radius: 20px;
  -webkit-border-bottom-right-radius: 20px;
  -moz-border-radius-topright: 20px;
  -moz-border-radius-bottomright: 20px;
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
  color: white;
}
button:hover {
  color: red;
  background-color: #ffffff;
}

.cards-pokemon {
  display: flex;
  align-items: flex-start;
  flex-direction: row;
  background-color: #ffcb32;
  width: 100%;
  height: 800px;
}

.tarjet {
  /*     border:2px solid #B1D75C;
    border-radius: 12px; */
  height: auto;
  width: auto;
  max-width: 300px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: black;
  margin: 15px;
  font-family: "Nunito", sans-serif;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}
.tarjet img {
  width: 100%;
  height: 80%;
  object-fit: cover;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}

.Title_card {
  margin-top: 10px;
  font-size: 1.6rem;
  padding: 2px 12px;
  background-color: #1ef30d;
  box-shadow: 3px 3px 0px #acf5da;
  border: 1px solid #000000;
}

.description-tarjet {
  width: 100%;
  padding: 0 20px;
  font-size: 1.2rem;
  color: white;
  margin: 14px;
  font-size: 1.4rem;
}
.normal {
  background-color: $normal;
}
.fire {
  background-color: $fire;
}
.water {
  background-color: $water;
}
.grass {
  background-color: $grass;
}
.electric {
  background-color: $electric;
}
.ice {
  background-color: $ice;
}
.fighting {
  background-color: $fighting;
}
.poison {
  background-color: $poison;
}
.ground {
  background-color: $ground;
}
.flying {
  background-color: $flying;
}
.psychic {
  background-color: $psychic;
}
.bug {
  background-color: $bug;
}
.rock {
  background-color: $rock;
}
.ghost {
  background-color: $ghost;
}
.dark {
  background-color: $dark;
}
.dragon {
  background-color: $dragon;
}
.steel {
  background-color: $steel;
}
.fairy {
  background-color: $fairy;
}
</style>
