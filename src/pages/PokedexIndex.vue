<template>
  <q-page class="q-pt-md q-px-sm">
    <div class="row justify-center">
      <pokeCard
        class="col-2 q-mx-sm q-mb-md pulse-grow-on-hover"
        style="width: 15%"
        v-for="pokemon in pokemons"
        :key="pokemon.name"
        :pokemon="pokemon"
        @click="teste(pokemon.name)"
      />
    </div>
    <q-footer elevated fixed class="bg-red-9">
      <q-toolbar class="justify-center">
        <q-btn
          v-for="button in buttons"
          :key="button.num"
          size="md"
          :label="button.num"
          class="q-mx-xs text-black"
          :class="[button.selected ? 'bg-primary' : 'bg-grey-6']"
          @click="paginationHandleClick(button)"
        />
      </q-toolbar>
    </q-footer>
  </q-page>
</template>

<script>
import axios from "axios";
import pokeCard from "../components/pokeCard.vue";
import { Notify, Loading } from "quasar";

export default {
  name: "PokedexIndex",

  components: { pokeCard },

  data() {
    return {
      pokemons: [],
      result: {},
      buttons: [],
      page: 1,
    };
  },

  async mounted() {
    Loading.show({ message: "Buscando..." });
    let qtdButtons = parseInt(Math.ceil(1010 / 36));
    this.getPokemons();
    for (let i = 1; i <= qtdButtons; i++) {
      this.buttons.push({
        num: i,
        selected: i == this.page ? true : false,
      });
    }
    Loading.hide();
  },

  methods: {
    paginationHandleClick(button) {
      Loading.show({ message: "Buscando..." });
      this.buttons.map((e) => {
        if (e.num == button.num) {
          this.page = button.num;
          e.selected = true;
          return e;
        }
        e.selected = false;
        return e;
      });
      this.getPokemons();
    },
    getPokemons() {
      axios
        .get(
          `https://pokeapi.co/api/v2/pokemon?offset=${
            (this.page - 1) * 36
          }&limit=${this.page * 36 <= 1010 ? 36 : 36 - (29 * 36 - 1010)}`
        )
        .then((resp) => {
          this.pokemons = resp.data.results;
          this.result = resp.data;
        })
        .finally(() => {
          Loading.hide();
        });
    },
  },
};
</script>

<style>
@-webkit-keyframes pulse-grow-on-hover {
  to {
    -webkit-transform: scale(1.1);
    transform: scale(1.1);
  }
}
@keyframes pulse-grow-on-hover {
  to {
    -webkit-transform: scale(1.1);
    transform: scale(1.1);
  }
}
.pulse-grow-on-hover {
  cursor: pointer;
  display: inline-block;
  vertical-align: middle;
  -webkit-transform: perspective(1px) translateZ(0);
  transform: perspective(1px) translateZ(0);
  box-shadow: 0 0 1px rgba(0, 0, 0, 0);
}
.pulse-grow-on-hover:hover,
.pulse-grow-on-hover:focus,
.pulse-grow-on-hover:active {
  -webkit-animation-name: pulse-grow-on-hover;
  animation-name: pulse-grow-on-hover;
  -webkit-animation-duration: 0.5s;
  animation-duration: 0.5s;
  -webkit-animation-timing-function: linear;
  animation-timing-function: linear;
  -webkit-animation-iteration-count: infinite;
  animation-iteration-count: infinite;
  -webkit-animation-direction: alternate;
  animation-direction: alternate;
}
</style>
