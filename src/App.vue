<template>
  <header>
    <h1>Pokedex</h1>
  </header>

  <label class="searchBar-label">
    <input 
    type="text" 
    v-model="pokemonName" 
    placeholder="Digite aqui o nome do Pokemon"
    >
    <button @click="searchPokemon">Procurar</button>
  </label>

  <div 
  class="main"
  v-if="Object.entries(pokemonData).length > 0">
    <section class="pokemonCard-container">
      <div class="pokemonCard-nameImg">
        <h1 class="pokemonCard-name">{{ pokemonData.name }}</h1>
        <img 
        :src="pokemonData.sprites.front_default" 
        alt="pokemonData.name"
        >
      </div>
    </section>
  </div>


</template>

<script>
import { pokeApi } from "./api/PokeApi.js"

export default {
  name: 'App',

  data() {
    return {
      pokemonData: {},
      pokemonName: '',

    }
  },

  methods: {
    async searchPokemon() {
      try {
        const pokemonToFind = await fetch(`${pokeApi}/${this.pokemonName.toLowerCase()}`)
        const pokemon = await pokemonToFind.json()
        this.pokemonData = pokemon
        console.log(pokemon);
        return pokemon

      } catch (error) {
        alert("Pokemon não encontrado")
      }
    }
  },
  
}
</script>
<style>
#app {
  font-family: Tahoma;
  text-align: center;
  margin-top: 60px;
}
</style>
