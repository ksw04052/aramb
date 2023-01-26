<script>

import championData from './data/champion.json'

export default {
  name : 'App',

  data(){
    return {
      championData,
      deck: [],
      tn: 10
    }
  },

  filters : {  
    
  },

  methods: {
    shuffle() {
      this.deck = this.array
      for (let index = this.deck.length - 1; index > 0; index--) {
        const randomPosition = Math.floor(Math.random() * (index + 1));

        const temporary = this.deck[index];
        this.deck[index] = this.deck[randomPosition];
        this.deck[randomPosition] = temporary;
      }
    },

    en(i) {
      return Object.keys(championData[0])[i]
    },

    kr(i) {
      return this.champs[this.en(i)].name
    },

    src(i) {
      return this.champs[this.en(i)].image.full
    }
  },

  computed: {
    champs() {
      return championData[0]
    },

    array() {
      return Array.from(Array(162).keys())
    },

    first() {
      return Math.ceil(this.tn/2)
    },

    second() {
      return this.tn - this.first
    }
    

  },

  components: {
    
  },

  created() {
    this.shuffle()
  }

}

</script>

<template>
  <div>
    <div>
      <h1>칼바람 내전 랜덤 조합 메이커</h1>
    </div>
    <div>
      <span class="input">팀당 챔피언:</span>
      <input type="number" min=1 max=80 class="input" v-model="tn">
      <button class="button" @click="shuffle">새 게임</button>
    </div>
    <div>
      <h2>Team 1</h2>
    </div>
    <div v-for="i in first" :key="i" class="champion">
      <img :src="`./img/${src(deck[i])}`" class="logo" alt="">
      <span class="label">{{kr(deck[i])}}</span>
    </div>
    <br>
    <div v-for="i in second" :key="i" class="champion">
      <img :src="`./img/${src(deck[i+first])}`" class="logo" alt="">
      <span class="label">{{kr(deck[i+first])}}</span>
    </div>
    <br>
    <div>
      <h2>Team 2</h2>
    </div>
    <div v-for="i in first" :key="i" class="champion">
      <img :src="`./img/${src(deck[i+81])}`" class="logo" alt="">
      <span class="label">{{kr(deck[i+81])}}</span>
    </div>
    <br>
    <div v-for="i in second" :key="i" class="champion">
      <img :src="`./img/${src(deck[i+81+first])}`" class="logo" alt="">
      <span class="label">{{kr(deck[i+81+first])}}</span>
    </div>
  </div>

</template>

<style scoped>
@font-face {
  font-family:'gabia_solmee';
  src: url('/fonts/gabia_solmee.ttf') format('truetype');
  font-weight: 700;
}
div {
  font-family:'gabia_solmee';
  text-shadow: -1px 0 rgb(255, 255, 255), 0 1px rgb(255, 255, 255), 1px 0 rgb(255, 255, 255), 0 -1px rgb(255, 255, 255);
}
.button {
  position: relative;
  bottom: 5px;
}
.champion {
  vertical-align: top;
  display: inline-block;
  text-align: center;
}
.label {
  display: block;
  font-size: 18px;
}
.input {
  font-size: 30px;
  width: 60px;
}

</style>
