<template>
  <div>
    <div class="column is-half is-offset-one-quarter">
      <img id="img" src="./assets/pokemon.png">
      <h4 class="subtitle is-size-4">Pokedex</h4>
      <input id="placeholder" class="input is-rounded is-warning" type="text" placeholder="Buscar pokémon pelo nome"
        v-model="busca">
      <button id="buscaBtn" class="button is-fullwidth is-rounded is-danger" @click="buscar">Buscar</button>
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon'
export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  // Aula 1: Aprendi a usar o created
  created: function () {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      console.log("Pegou a lista de pokemons");
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results
    })
  },
  components: {
    Pokemon
  },
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons // resetanto a lista de pokemons filtrados
      if (this.busca == '' || this.busca == ' ') {
        this.filteredPokemons = this.pokemons
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
  },
  computed: {
    // resultadoBusca: function() {
    //   if(this.busca == '' || this.busca == ' ') {
    //     return this.pokemons
    //   } else {
    //     return this.pokemons.filter(pokemon => pokemon.name == this.busca)
    //   }
    // }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#img {
  margin-top: 2%;
  border-radius: 4em;
}

#placeholder {
  text-align: center;
}

#buscaBtn {
  margin-top: 2%;
}

</style>