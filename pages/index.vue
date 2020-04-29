<template>
  <div class="container">
    <div>
      <h1 class="title">
        <span>Sensing</span>
        Blue
      </h1>
      <h2 class="subtitle">
        The role of flavor perception in modern blueberry breeding
      </h2>
      <landing />
      <h5 class="scroll_text">Scroll</h5>
      <div class="tri_override">
        <triangle />
      </div>
      <div class='icon_scroll'></div>
      <article class='intro_text'>

      </article>
      <div class="sensory_scroll">
        <scrolly />
      </div>
      <div class ="radial_chart">
        <radial v-for="(variety,i) in radialData" 
        :key="i" 
        :id="i" 
        :allData="radialData"/>
        <radial :allData="radialData"/>
      </div>  
      </div>
        <div class ="bar_graph">
        <bar />
      </div>
  </div>
</template>

<script>
import landing from '~/components/Logo.vue'
import triangle from '~/components/triangle_scroll.vue'
import scrolly from '~/components/scrolly.vue'
import bar from '~/components/bar.vue'
import radial from '~/components/radial.vue'
import radialData from "~/assets/radialBerry.json"
export default {
  data(){
    return{
      radialData:radialData
    }
  },
  components: {
    landing,
    triangle,
    scrolly,
    bar,
    radial,
    radialData
  }
}

</script>

<style>
.container {
  margin: 0 auto;
  justify-content: center;
  align-items: center;
  text-align: center;
  background-color: #fffcf6;
}

.title span {
  font-family: 'DM Serif Display';
  font-weight: 300;
  font-size: 120px;
  -webkit-text-fill-color: white;
  -webkit-text-stroke-width: 1px;
  -webkit-text-stroke-color: #0b4780;
  stroke-width: 5px;
  letter-spacing: 1px;
  padding-top: 30px;
  padding-bottom: 30px;
}

.title {
  font-family: 'DM Serif Display';
  font-weight: 300;
  font-size: 120px;
  -webkit-text-fill-color: #0b4780;
  -webkit-text-stroke-width: 0px;
  letter-spacing: 1px;
  padding-top: 10px;
  padding-bottom: 0px;
}

.subtitle {
  font-family: 'DM Serif Display';
  font-style: italic;
  font-weight: 400;
  font-size: 22px;
  -webkit-text-fill-color: #0b4780;
  letter-spacing: 1px;
  word-spacing: 10px;
  padding-bottom: 15px;
  padding-top: 0px;
}

.bar_graph {
  justify-content: center;
  align-items: center;
}

.radial_chart {
 display: flex;
 flex-wrap: wrap;
 align-content: center;
 justify-content: center;
}

.scroll_text{
  font-family: 'DM Serif Display';
  font-style: italic;
  font-size: 14px;
  -webkit-text-fill-color: #0b4780;
  letter-spacing: 1px;
  /* word-spacing: 10px; */
  padding-bottom: 0px;
  padding-top: 10px;
}

.tri_override{
  color: #0b4780;
  width: 35px;
  height: 40px;
  margin: 0 auto;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.icon_scroll,
.icon_scroll:before{
  position: absolute;
  left: 50%;
}

.icon_scroll{
  width: 40px;
  height: 70px;
  margin-left: -19px;
  top: 85%;
  margin-top: 30px;
  /* box-shadow: inset 0 0 0 1px #000; */
  border-radius: 25px;
}
.icon_scroll:before{
  content: '';
  width: 5px;
  height: 5px;
  background: #0b4780;
  margin-left: -4px;
  top: 35px;
  border-radius: 2px;
  animation-duration: 1.5s;
  animation-iteration-count: infinite;
  animation-name: scroll;
}

.sensory_scroll{
  padding-top: 20px;
}

    @keyframes scroll {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
    transform: translateY(20px)
  }
}

</style>
