<template>
  <ul class="ul-container">
    <div v-for="pokemon in currentPagePoke" :key="pokemon.id">
      <a
        target="_blank"
        :href="`https://bulbapedia.bulbagarden.net/wiki/${pokemon.name}`"
        rel="noreferrer"
      >
        <li>{{ pokemon.name }}</li>
      </a>
      <img
        :alt="pokemon.name"
        :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
          isShiny ? 'shiny' : ''
        }/${pokemon.id}.png`"
      />
    </div>
  </ul>
</template>

<script>
import { EventBus } from "../main.js";
export default {
  name: "List",
  data() {
    return {
      pokemons: [],
      filerPokemons: [],
      currentPagePoke: [],
      filterKeyword: "",
      gen: "",
      isShiny: false,
      pokeIndex: [0, 18],
    };
  },
  props: ["pokePerPage"],
  methods: {
    getPokemonList(gen) {
      fetch(`https://pokeapi.co/api/v2/generation/${gen}`)
        .then((res) => res.json())
        .then((json) => {
          json.pokemon_species.map((item) => {
            // add Dex# as the id as item attribute to the list
            // the url of each poke is 'https://pokeapi.co/api/v2/pokemon-species/1/'.  The Dex# is the 7th item
            item.id = item.url.split("/")[6];
            return item;
          });
          //the api fetch should be called only when page or new Gen is loaded
          //The result is unsorted list of Pokemons, so need to sort them by id
          const compare = (a, b) => {
            if (parseInt(a.id) < parseInt(b.id)) {
              return -1;
            }
            if (parseInt(a.id) > parseInt(b.id)) {
              return 1;
            }
            return 0;
          };
          console.log("New pokemon List is fetched");
          //When each Generation of Pokemon is loaded. Initiate the list of Pokemon (all poke, filter poke and current page of poke)
          this.pokemons = json.pokemon_species.sort(compare);
          this.filerPokemons = json.pokemon_species.sort(compare);
          EventBus.$emit("total-filter-pokemon", this.filerPokemons.length);
          this.currentPagePoke = json.pokemon_species
            .sort(compare)
            .slice(this.pokeIndex[0], this.pokeIndex[1]);
        });
    },
  },
  watch: {
    filterKeyword() {
      const comparePokemon = this.pokemons.filter((item) => {
        return item.name.includes(this.filterKeyword.toLowerCase());
      });
      this.filerPokemons = comparePokemon;
      EventBus.$emit("total-filter-pokemon", this.filerPokemons.length);
      this.currentPagePoke = this.filerPokemons.slice(
        this.pokeIndex[0],
        this.pokeIndex[1]
      );
    },
    gen() {
      this.getPokemonList(this.gen);
    },
    pokeIndex() {
      this.currentPagePoke = this.filerPokemons.slice(
        this.pokeIndex[0],
        this.pokeIndex[1]
      );
    },
  },
  created() {
    this.getPokemonList("1");
  },
  mounted() {
    EventBus.$on("update-filter", (value) => {
      this.filterKeyword = value;
    });
    EventBus.$on("update-gen", (value) => {
      this.gen = value;
    });
    EventBus.$on("update-shiny", (value) => {
      this.isShiny = value;
    });
    EventBus.$on("update-current-page", (value) => {
      this.pokeIndex = value;
    });
  },
};
</script>

<style scoped>
.ul-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  list-style: none;
  text-align: center;
  background-color: lightcyan;
}

img {
  border-radius: "10px";
  border: "10px solid #555";
}
</style>