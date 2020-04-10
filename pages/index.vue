<template>
  <div class="container">
    <div>
      <logo />
      <h1 class="title">
        Sensing Berries
      </h1>
      <h2 class="subtitle">
        Bluberry Sensory Analysis
      </h2>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          class="button--green"
        >
          Documentation
        </a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          class="button--grey"
        >
          GitHub
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Times New Roman';
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #000000;
  letter-spacing: 1px;
}

.subtitle {
  font-family: 'Times New Roman';
  font-weight: 300;
  font-size: 42px;
  color: #000000;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
