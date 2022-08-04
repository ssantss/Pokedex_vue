<template>
  <div class="tarjet">
    <img :src="pokemon.sprites.front_default" :alt="pokemon.name" />
    <h2 class="Title_card">{{ pokemon.name }}</h2>
    <div class="description-tarjet">
      <p><b>Tipo </b></p>
      <p
        v-for="(type, index) in pokemon.types"
        :key="index"
        :class="type.type.name"
      >
        {{ type.type.name }}
      </p>
      <p><b>Estadisticas </b></p>
      <p v-for="(stat, index) in pokemon.stats" :key="index">
        {{ stat.stat.name }} → {{ stat.base_stat }}
      </p>
    </div>
  </div>
</template>
<script>
export default {
  name: "PokemonCard",
  props: {
    pokemon: {
      type: Object,
      required: true,
    },
  },
};
</script>
