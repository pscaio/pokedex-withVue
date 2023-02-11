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
    <button @click="searchPokemon()">Procurar</button>
  </label>

  <div 
  class="main"
  v-if="Object.entries(pokemonData).length > 0">
    <section class="pokemonCard-container">
      <div class="pokemonCard-nameImg">
        <h1 class="pokemonCard-name">{{ pokemonData.name }}</h1>    
        <img 
        :src="pokemonData.sprites.front_default" 
        :alt="pokemonData.name"
        >
      </div>

      <div class="pokemonCard-info">
        <ul class="pokemonCard-info_type">
          <h2>Type:</h2>
          <li
          v-for="(type, index) in pokemonData.types"
          :key="index"
          :class="type.type.name"
          >
            <span>{{ type.type.name }}</span>
          </li>
        </ul>
        <ul class="pokemonCard-info_stats">
          <h2>Stats:</h2>
            <li
            v-for="(stats, index) in pokemonData.stats"
            :key="index"
            >
              <span>{{stats.stat.name}} ➞ {{ stats.base_stat }} </span>
            </li>
        </ul>
      </div>
    </section>
  </div>


</template>

<script>
import { pokeApi, /* pokeApiSpecies */} from "./api/PokeApi.js"


export default {
  name: 'App',

  data() {
    return {
      pokemonData: {},
      //specieData: {},
      pokemonName: '',
      //specieID: '',
    }
  },

 

  methods: {
    async searchPokemon() {
      try {
        const pokemonToFind = await fetch(`${pokeApi}/${this.pokemonName.toLowerCase()}`)
        const pokemon = await pokemonToFind.json()
        this.pokemonData = pokemon
      } catch (error) {
        alert(error)
      }
    },
    
    //tentativa falha de pegar o ID atravez da url base 'pokeApi' para atribuir em uma nova onde existe as especies para apos isso atribuir as evoluções e mostrar na tela
   /* async speciesUrl() {
      try {
        const specieToFind = await fetch(`${pokeApiSpecies}/${this.specieID}`)
        
        const specie = await specieToFind.json()
        this.specieData = specie
        this.getSpeciesID()
        return specie
      } catch (error) {
        console.log(error);
      }
    },

     getSpeciesID() {
      const pokemonSpeciesUrl = this.pokemonData.species.url
      this.specieID = pokemonSpeciesUrl.split('/')[6],
      
      console.log("getSpeciesID", this.specieID);
      return this.specieID
    }, */
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
