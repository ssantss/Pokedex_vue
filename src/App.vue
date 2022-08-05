<template>
  <body class="container_main">
    <header class="header">
      <div class="logo">
        <img src="@/assets/pngwing.com.png" alt="Logo_pokemon" />
      </div>
      <p>BUSCA TU POKEMON</p>
      <label>NOMBRE, TIPO o ID:</label>
      <div class="searchcontainer">
        <input
          type="text"
          placeholder="Escribe aqui!"
          class="pokemonSearch"
          v-model="pokemonID"
          @keyup.enter="searchPokemon"
        />
        <button @click="searchPokemon" class="searchpokemon">
          Search pokemon!
        </button>
      </div>
      <div class="conteinerSorprendeme">
        <button class="buttom_sorprendeme" @click="random">Sorprendeme</button>
      </div>
      <div class="container_logos">
        <div class="logo_github">
          <a href="https://github.com/ssantss/Pokedex_vue" target="_blank"
            ><img src="./assets/github-icon.png" alt="github icon"
          /></a>
        </div>
        <div class="logo_platzi">
          <a href="https://platzi.com/p/saants/" target="_blank"
            ><img src="./assets/logotipo-platzi.png" alt="platzi icon"
          /></a>
        </div>
      </div>
    </header>
    <div class="containercards">
      <section class="containercards" v-if="pokemons">
        <div v-for="(pokemon, i) in pokemons" :key="i" class="cards-pokemon">
          <PokemonCard :pokemon="pokemon" @removePokemon="removePokemon" />
        </div>
      </section>
    </div>
  </body>
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
        this.$swal({
          position: "center",
          icon: "error",
          showConfirmButton: false,
          title: "POKEMON NO ENCONTRADO!",
          timer: 800,
        });
        console.log(error);
      }
    },
    async searchPokemonRadom(randomNumber) {
      try {
        const pokemonID = randomNumber;
        const pokemonToFind = await fetch(`${pokeapi}${pokemonID}`);
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
      const pokemonIndex = this.pokemons.findIndex((p) => p.id === pokemon.id);
      if (pokemonIndex === -1) {
        this.pokemons.unshift(pokemon);
      }
    },
    removePokemon(id) {
      const index = this.pokemons.findIndex((pokemon) => pokemon.id === id);
      this.pokemons.splice(index, 1);
    },
    random() {
      const maxrandom = 14;
      for (let index = 0; index < maxrandom; index++) {
        const minPokemon = 0;
        const maxPokemon = 905;
        const randomNumber = Math.floor(
          Math.random() * (maxPokemon - minPokemon + 1) + minPokemon
        );
        console.log("RANDOM = ", randomNumber);
        this.searchPokemonRadom(randomNumber);
      }
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
/* container_main {
  width: 100%;
  height: 100%;
} */

.header {
  display: flex;
  flex-direction: column;
  width: auto;
  height: auto;
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
.buttom_sorprendeme {
  width: 200px;
  height: 40px;
  background-color: red;
  border: none;
  letter-spacing: 0.5px;
  cursor: pointer;
  font-size: 13px;
  font-family: "poke";
  margin: 10px;
  color: white;
  border-radius: 20px;
}
.buttom_sorprendeme:hover {
  color: red;
  background-color: #ffffff;
}
.searchpokemon {
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
.searchpokemon:hover {
  color: red;
  background-color: #ffffff;
}
.container_logos {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}
.logo_github img {
  width: 30px;
  height: 30px;
  cursor: pointer;
}
.logo_platzi img {
  width: 51.5px;
  height: 25px;
  cursor: pointer;
}

.containercards {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
  font-family: "Nunito", sans-serif;
  width: auto;
  min-height: 700px;
  overflow: auto;
  height: auto;
  margin: 0;
  align-items: flex-start;
  background-color: #ffcb32;
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
  border-top-left-radius: 50px;
  border-top-right-radius: 50px;
  border-bottom-left-radius: 50px;
  border-bottom-right-radius: 50px;
  transition: all 0.2s ease-in-out;
}
.tarjet:hover {
  transform: scale(1.05);
}
.tarjet img {
  width: 100%;
  height: 80%;
  object-fit: cover;
  border-top-left-radius: 50px;
  border-top-right-radius: 50px;
}

.Title_card {
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
  text-align: center;
}

.buttom_eliminar {
  box-sizing: border-box;
  position: relative;
  display: block;
  transform: scale(var(--ggs, 1));
  width: 22px;
  height: 22px;
  border: 2px solid;
  border-radius: 22px;
  cursor: pointer;
  margin-bottom: 5px;
}

.buttom_eliminar::before {
  content: "";
  display: block;
  box-sizing: border-box;
  position: absolute;
  width: 10px;
  height: 2px;
  background: currentColor;
  border-radius: 5px;
  top: 8px;
  left: 4px;
}

.normal {
  background-color: $normal;
  border-radius: 20px;
  margin: 3px;
}
.fire {
  background-color: $fire;
  border-radius: 20px;
  margin: 3px;
}
.water {
  background-color: $water;
  border-radius: 20px;
  margin: 3px;
  margin: 3px;
}
.grass {
  background-color: $grass;
  border-radius: 20px;
  margin: 3px;
}
.electric {
  background-color: $electric;
  border-radius: 20px;
  margin: 3px;
}
.ice {
  background-color: $ice;
  border-radius: 20px;
  margin: 3px;
}
.fighting {
  background-color: $fighting;
  border-radius: 20px;
  margin: 3px;
}
.poison {
  background-color: $poison;
  border-radius: 20px;
  margin: 3px;
}
.ground {
  background-color: $ground;
  border-radius: 20px;
  margin: 3px;
}
.flying {
  background-color: $flying;
  border-radius: 20px;
  margin: 3px;
}
.psychic {
  background-color: $psychic;
  border-radius: 20px;
  margin: 3px;
}
.bug {
  background-color: $bug;
  border-radius: 20px;
  margin: 3px;
}
.rock {
  background-color: $rock;
  border-radius: 20px;
  margin: 3px;
}
.ghost {
  background-color: $ghost;
  border-radius: 20px;
  margin: 3px;
}
.dark {
  background-color: $dark;
  border-radius: 20px;
  margin: 3px;
}
.dragon {
  background-color: $dragon;
  border-radius: 20px;
  margin: 3px;
}
.steel {
  background-color: $steel;
  border-radius: 20px;
  margin: 3px;
}
.fairy {
  background-color: $fairy;
  border-radius: 20px;
  margin: 3px;
}
</style>
