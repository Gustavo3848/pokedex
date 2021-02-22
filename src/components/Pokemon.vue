<template>
  <div id="pok">
    <div class="card">
      <div class="card-image">
        <figure>
          <img v-if="isFront" :src="pokemon.frontSprite"/>
          <img v-else :src="pokemon.backSprite"/>
        </figure>
      </div>
      <div class="card-content">
        <div class="media-content">
          <p class="title is-4">{{ pokemon.nome | upper }}</p>
          <p class="title is-6">{{pokemon.tipo}}</p>
        </div>
        <div class="content"><button @click="changeSprite" class="button is-normal">Mudar Sprite</button></div>
      </div>
    </div>
  </div>
</template>

<script>
import Axios from "axios";
export default {
  data() {
    return {
      currentSprite: null,
      isFront:true,
      pokemon:{
        nome: this.pok.name,
        frontSprite: null,
        backSprite: null,
        tipo: null,
        url: this.pok.url
      }
    };
  },
  props: {
    pok:Object,
    busca: String
  },
  filters: {
    upper: function (value) {
      var newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
  mounted() {
    Axios.get(this.pokemon.url).then((response) => {
      this.pokemon.frontSprite = response.data.sprites.front_default;
      this.pokemon.backSprite = response.data.sprites.back_default;
      this.currentSprite = response.data.sprites.front_default;
      this.pokemon.tipo = response.data.types[0].type.name;
    });
  },
  methods:{
    changeSprite: function(){
      if(this.isFront == true){
        this.isFront = false
      }else{
        this.isFront = true
      }
      console.log(this.currentSprite)
    }
  },
  
};
</script>

<style>
#pok {
  text-align: center;
}
.card{
    margin-top: 10px;
}
.content{
  margin-top: 2%;
}
</style>