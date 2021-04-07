<template>
  <div id="app">
    <Navbar/>
    <div class="column is-half is-offset-one-quarter">
      <input class="input is-rounded" type="text" placeholder="Buscar pokemon" v-model="pokemon">
      <button class="button is-success is-normal" id="btnBuscar" @click="buscar"> Buscar</button>
      <h4 v-show="SearchFail" id="Search">Nenhum Pokemon encontrado! :(</h4>
       <div v-for="(poke,index) in filterPokemons" :key="poke.url" >
      <Pokemon :name="poke.name" :url="poke.url" :index="index +1" />
   </div>
    </div>
  </div>
</template>

<script>

import axios from "axios";
import Pokemon from "./components/pokemon"
import Navbar from "./components/navbar"
export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filterPokemons: [],
      pokemon: "",
      SearchFail: false
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
    .then(result => {
      this.pokemons = result.data.results;
      this.filterPokemons = result.data.results;
    })
    .catch(err => console.log(err));

    //todoo: recebi os dados da api do poke, agora é só prosseguir
  },

  components: {
    Pokemon,
    Navbar
  },

  methods: {
    buscar: function(){
      console.log("oi");
      this.filterPokemons = this.pokemons; 
      if(this.pokemon == "" || this.pokemon == " "){
          this.filterPokemons = this.pokemons
          this.SearchFail = false;
         
      } else{
        let newList = this.filterPokemons.filter(poke => poke.name == this.pokemon);
        this.filterPokemons = newList;
        this.SearchFail = false;

        if(this.filterPokemons.length == 0){
          this.SearchFail = true
        }
      }
    }
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

#btnBuscar{
  margin-top: 2%;
}

#Search{
  margin-top: 4%;
}
</style>
