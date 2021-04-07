<template>
  <div id="pokemon">
      <div class="card">
          <div class="card-image">
            <figure>
              <img :src="pokeInfo.currentImage" alt="Placeholder image">
            </figure>
          </div>
          <div class="card-content">
            <div class="media">
              <div class="media-content">
                <p class="title is-4"> {{index}} {{name | upper}}</p>
                <p class="subtitle is-6"> {{pokeInfo.type}} </p>
                <button class="button is-success is-normal" @click="mudarSprite">Mudar Sprite</button>
              </div>
            </div>

            <div class="content">
              
        </div>
  </div>
</div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  created: function(){
      axios.get(this.url).then(res => {
        
        this.pokeInfo.type = res.data.types[0].type.name;
        this.pokeInfo.front = res.data.sprites.front_default;
        this.pokeInfo.back = res.data.sprites.back_default;

        this.pokeInfo.currentImage = res.data.sprites.front_default;


      })
  },
  data(){
    return {
      pokeInfo: {
        isFront: true,
        currentImage: "",
        type: "",
        front: "",
        back: ""
      }
    }
  },

 props: {
   name: String,
   url: String,
   index: Number
 },
 filters: {
   upper: function(value){
     //pega a primeira letra da string e aumenta ela, junto com a string inteira com a primeira letra cortada
     let newLetter = "- " + value[0].toUpperCase() + value.slice(1);

     return newLetter
   }
 },

 methods: {
   mudarSprite: function(){
     if(this.pokeInfo.isFront){
       // troca a posição da imagem do pokemon
       this.pokeInfo.isFront = false;
       this.pokeInfo.currentImage = this.pokeInfo.back;
     } else{
       this.pokeInfo.isFront = true;
       this.pokeInfo.currentImage = this.pokeInfo.front;
     }
   }
 }
}
</script>

<style>
#pokemon{
  margin-top: 3%;
}
</style>