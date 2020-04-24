<template>
  <div class="container">
    <div>
      <h1 class="title">
        Sensing Blue
      </h1>
      <logo />
      <!-- <scrolly /> -->
      <!-- <h2 class="subtitle">
        Bluberry Sensory Analysis
      </h2> -->
      <div class ="radial_chart">
        <radial v-for="(variety,i) in radialData" 
        :key="i" 
        :id="i" 
        :allData="radialData"/>
        <radial :allData="radialData"/>
      </div>  
      </div>
        <div class ="bargraph">
        <bar />
      </div>
      <div>
        <scrolly />
        <bar />
      </div>
  </div>
</template>

<script>
import radialData from "~/assets/radialBerry.json"
import logo from '~/components/Logo.vue'
import scrolly from '~/components/scrolly.vue'
import bar from '~/components/bar.vue'
import radial from '~/components/radial.vue'
export default {
  data(){
    return{
      radialData:radialData
    }
  },
  components: {
    logo,
    scrolly,
    bar,
    radialData,
    radial
  }
}

</script>

<style>
.container {
  margin: 0 auto;
  /* min-height: 100vh;
  display: flex; */
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'DM Serif Display';
  display: block;
  font-weight: 300;
  font-size: 120px;
  color: #0b4780;
  letter-spacing: 1px;
  padding-top: 60px;
  padding-bottom: 30px;
}

.subtitle {
  font-family: 'DM Serif Display';
  font-weight: 300;
  font-size: 42px;
  color: #000000;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.bargraph {
  justify-content: center;
  align-items: center;
}

.radial_chart {
 display: flex;
 flex-wrap: wrap;
}

</style>
