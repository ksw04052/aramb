<script>

import championData from './data/champion.json'
import championOrder from './data/championOrder.json'
import MyModal from './components/MyModal.vue'

export default {
  name : 'App',

  data(){
    return {
      championData,
      championOrder,
      ban: [],
      deck: [],
      tn: 10,
      modal: false
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
      console.log(this.array)
    },

    en(i) {
      return Object.keys(championData[0])[i]
    },

    kr(i) {
      return this.champs[this.en(i)].name
    },

    src(i) {
      return this.champs[this.en(i)].image.full
    },

    manageBan(i) {
      let index = this.championOrder.findIndex(champ => champ.name == i)
      if(this.ban.includes(index)) {
        let idx = this.ban.findIndex(a => a == index)
        this.ban.splice(idx, 1)
      }
      else {
        this.ban.push(index)
        this.ban.sort()
      }
      console.log(this.ban)
    },

    openModal() {
      this.modal = true
    },

    closeModal() {
      this.modal = false
    },

    test() {
      console.log(this.championData)
    }
  },

  computed: {
    champs() {
      return championData[0]
    },

    array() {
      let all = Array.from(Array(164).keys())
      for (let i=0; i<this.ban.length; i++) {
        const idx = all.indexOf(this.ban[i])
        if (idx > -1) all.splice(idx, 1)
      }
      return all
    },

    first() {
      return Math.ceil(this.tn/2)
    },

    second() {
      return this.tn - this.first
    }
    

  },

  components: {
    MyModal
  },

  created() {
    this.shuffle()
  }

}

</script>

<template>
  <div>
    <MyModal v-if="modal" @close-modal="modal=false" @toggle-ban="manageBan" v-bind:ban="ban" />
    <div>
      <h1>칼바람 내전 랜덤 조합 메이커</h1>
    </div>
    <div>
      <span class="input">팀당 챔피언:</span>
      <input type="number" min=1 max=70 class="input" v-model="tn">
      <div>
        <button @click="openModal">밴</button>
        <button @click="shuffle">새 게임</button>
        <!-- <button @click="test">테스트</button> -->
      </div>
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
button {
  margin: 2px;
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
