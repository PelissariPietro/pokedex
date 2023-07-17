<template>
  <q-card class="my-card" :style="bgColor">
    <q-img :src="spriteLink">
      <div
        class="absolute-bottom text-subtitle2 text-center text-weight-bold"
        style="width: 100%"
      >
        {{ pokeId }} - {{ pokeName }}
      </div>
    </q-img>
  </q-card>
</template>

<script>
import axios from "axios";

export default {
  name: "pokeCard",
  props: ["pokemon"],

  data() {
    return {
      bgColor: "",
      spriteLink: "",
      pokeName: "",
      pokeId: Number,
      pokeInfo: {},
      pokeTypesColor: {
        bug: "#A8B820",
        dark: "#705848",
        dragon: "#7038F8",
        electric: "#F8D030",
        fairy: "#EE99AC",
        fighting: "#C03028",
        fire: "#F08030",
        flying: "#A890F0",
        ghost: "#705898",
        grass: "#78C850",
        ground: "#E0C068",
        ice: "#98D8D8",
        normal: "#A8A878",
        poison: "#A040A0",
        psychic: "#F85888",
        rock: "#B8A038",
        steel: "#B8B8D0",
        water: "#6890F0",
      },
    };
  },

  created() {
    this.pokeId = Number(this.pokemon.url.split("/")[6]);
    axios
      .get("https://pokeapi.co/api/v2/pokemon/" + this.pokeId)
      .then((resp) => {
        this.pokeInfo = resp.data;
        this.setBackgroundColor(resp.data.types);
      });
    this.spriteLink =
      "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/" +
      this.pokeId +
      ".png";
    this.pokeName =
      this.pokemon.name.substr(0, 1).toUpperCase() + this.pokemon.name.slice(1);
  },

  methods: {
    setBackgroundColor(types) {
      if (types.length == 1) {
        this.bgColor =
          "background-color: " + this.pokeTypesColor[types[0].type.name];
      } else if (types.length > 1) {
        let colors = [];
        types.forEach((type) => {
          colors.push(this.pokeTypesColor[type.type.name]);
        });
        this.bgColor =
          "background-image: linear-gradient(to right bottom, " +
          colors.join(",") +
          ")";
      }
    },
  },
};
</script>
