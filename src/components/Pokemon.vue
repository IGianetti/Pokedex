<template>
  <div class="container">
    <img :src="require('/src/assets/logo.png')" alt="pokedex" />

    <Search :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl" />
    <List
      :imageUrl="imageUrl"
      :apiUrl="apiUrl"
      @setPokemonUrl="setPokemonUrl"
    />
    <Details
      v-if="showDetail"
      :pokemonUrl="pokemonUrl"
      :imageUrl="imageUrl"
      @closeDetail="closeDetail"
    />
  </div>
</template>

<script>
import Search from "./Search.vue";
import List from "./List.vue";
import Details from "./Details.vue";
export default {
  data: () => {
    return {
      imageUrl:
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/",
      apiUrl: "https://pokeapi.co/api/v2/pokemon/",
      pokemonUrl: "",
      showDetail: false,
    };
  },
  components: {
    Search,
    List,
    Details,
  },
  methods: {
    setPokemonUrl(url) {
      this.pokemonUrl = url;
      this.showDetail = true;
    },
    closeDetail() {
      this.pokemonUrl = "";
      this.showDetail = false;
    },
  },
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css?family=Acme");
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 10px;
  width: calc(100% - 20px);
  min-height: calc(100vh - 20px);
  background: radial-gradient(#159941, #0a2e50);
  font-family: "Acme", arial;
  font-size: 1rem;
  font-weight: normal;
}
h1 {
  color: #efefef;
}
img {
  padding: 10px;
  width: 250px;
  height: 100px;
}
</style>
