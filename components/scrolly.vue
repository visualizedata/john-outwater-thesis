<template>
  <div>
    <client-only>
    <Scrollama :offset="0.5" @step-enter="stepEnterHandler" id="flexed">
      <div slot="graphic" class="graphic">
        <img :src="steps[currStepId-1].image" width="700"/>
      </div>
      <div
        v-for="step in steps" :key="step.no"
        :data-step-id="step.id"
        class="step" :class="{'is-active': step.id === currStepId}">
        <h2 class="scroll_title">{{step.title}}</h2>
        <p class="text">{{step.text}}</p>
      </div>
    </Scrollama>
    </client-only>
  </div>
</template>

<script>
// polyfill for IntersectionObserver
// import 'intersection-observer'
// import Scrollama from 'vue-scrollama'

export default {
  components: {
  },
  data() {
    return {
      currStepId: 1,
      steps: [
        {id: '1', title: "Appearance and Expectation", text: "Using vision to understand our food is one of the first interactions we have in the process of food consumption. In these situations, we judge if we want to consume a food or not, and sometimes we even ponder how good the food will taste based on its appearance. We are setting positive or negative expectations as we take a momentary but highly crucial and instinctual glance at the food prior to eating. Our pupils, irises, retinas and brain are an incredible system that can analyze very minute details in the texture and color of our environment, especially when we hold up food items close to our eyes. We possess information and experiences in our brains that relate our visual perception to what we have seen in the past and what we have learned as comparatively regular or irregular in appearance. Therefore, our eyes are the first step in building comprehensive flavor perception.", image: "./flav_eye.png"},
        {id: '2', title: "Taste Transduction",text: "Our mouths are one of the many sense organs in our bodies. It is of especially high significance as it is one of the pathways through which we breathe and it transforms products of the external world to digestible matter. During this transition point from external to internal, we use the tongue in our mouths to sense the taste and touch (mouth feel) of potentially ingestible food for our bodies. The moment food touches our tongue, receptors pass messages to the brain, giving us an understanding of whether the food is safe to eat and, furthermore, what the food is generally composed of. The tongue is able to sense five distinct tastes: sweet, sour, salt, bitter, and umami. These tastes are all sensed and transduced to the brain by taste receptor cells  located within various papillae along the tongue’s surface. Collectively, the TRCs start to build a perception of flavor during a consumption experience.", image: "./flav_mouth.png"},
        {id: '3', title: "Retronasal Flow",text: "The nose is a sensory organ that has also adapted in different ways from our early existence as humans. While this evolution and adaptation was occuring, our keen sense of smell transitioned from orthonasal smell to retronasal smell, which originates from our mouths through the back-end of our nasal cavity. When a piece of edible matter enters the mouth, the retronasal sense engages almost immediately. As one chews food and breathes, small whiffs of the food are transported from the back of the mouth up to the retronasal passage as a primer. When the food is swallowed, a natural breath-out allows a more intense experience of the food’s odor retronasally, where hundreds of volatile compounds can be translated and visualized by way of the olfactory bulb. This moment in consumption is when one experiencing food will have the most wholesome understanding of the food’s flavor.", image: "./flav_nasal.png"}
      ]
    }
  },
  methods: {
    stepEnterHandler({element, direction, index}) {
      console.log({element, direction, index});
      this.currStepId = element.dataset.stepId
    }
  }
}
</script>

<style src="vue-scrollama/dist/vue-scrollama.css"></style>

<style lang="scss">
// overrides here
#scrollama-container-flexed {  // id="flexed" passed as prop to Scrollama in template
  display: flex;
  flex-direction: row-reverse;
  .scrollama-graphic {
    flex: 2.0;
    height: 80vh;
    top: 10vh;;
  }
  .scrollama-steps {
    flex: 1;
  }
}
</style>

<style scoped>
.step {
  width: 100%;
  padding: 4rem 0;
  margin: 5rem 0rem 15rem;
  /* border: 1px solid #0b4780;
  background-color: white; */
  display: block;
  justify-content: left;
}
.step.is-active {
  background-color: none;
}

.scroll_title{
  font-family: 'DM Serif Display';
  font-weight: 400;
  font-size: 29px;
  -webkit-text-fill-color: #0b4780;
  padding: 0 3rem 1rem;
  text-align: left;
}

.text {
  font-family: 'HelveticaNeue-Light';
  /* font-style: "Light"; */
  font-weight: 400;
  font-size: 17px;
  padding: 0 3rem 0;
  text-align: justify;
  text-justify: inter-word;
}
.graphic {
  height: 80vh;
  width: 100%;
  /* border: 1px solid #333; */
  margin: 0 0rem;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 5rem;
}
</style>