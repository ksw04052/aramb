<template>
    <div class="modal">
      <div class="overlay" @click="$emit('close-modal')">
        
      </div>
      
      <div class="modal-card">
        <div v-for="(champ, index) in champs" :key="champ" @click="$emit('toggle-ban', index)" :class="{ border: isBanned(index)}">
          <img :src="`./img/${champ.image.full}`" class="logo" alt="" :class="{ gray: isBanned(index)}">

          <!-- <span class="label">{{kr(deck[i])}}</span> -->
        </div>
      </div>
    </div>
</template>

<script>
import championData from '../data/champion.json'
import championOrder from '../data/championOrder.json'

export default {
  data(){
    return {
      championData,
      championOrder
    }
  },

  props: {
    ban : Array
  },

  methods: {

    en(i) {
      return Object.keys(championData[0])[i]
    },

    kr(i) {
      return this.champs[this.en(i)].name
    },

    src(i) {
      return this.champs[this.en(i)].image.full
    },

    isBanned(index) {
      let n = this.championOrder.findIndex(champ => champ.name == index)
      if (this.ban.includes(n)) {
        return true
      }
      else {
        return false
      }
    }

  },

  computed: {
    champs() {
      return championData[0]
    },
    

  },
}
</script>

<style>
.modal,
.overlay {
    width: 100%;
    height: 100%;
    position: fixed;
    left: 0;
    top: 0;
}

.overlay {
    opacity: 0.5;
    background-color: black;
}

.modal-card {
    position: relative;
    max-width: 80%;
    margin: auto;
    margin-top: 30px;
    padding: 20px;
    background-color: white;
    min-height: 800px;
    z-index: 10;
    opacity: 1;
    overflow: scroll;
    display: flex;
    flex-wrap: wrap;
}

div {
  margin: 0;
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

.logo {
  height: 50px;
  width: 50px;
}

.gray {
  filter: grayscale(100%);
  height: 48px;
  width: 48px;
}

.border {
  border-width: 1px;
  border-style: solid;
  border-color: red;
  height: 48px;
}

</style>