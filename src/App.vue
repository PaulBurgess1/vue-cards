<template>
  <div class="container-fluid gx-0 p-0 m-0 w-100" id="app">
    <Header></Header>
      <HighOrLow></HighOrLow>
    <Footer></Footer>
  </div>
</template>

<script>
import HighOrLow from './components/HighOrLow.vue'
import Footer from './components/Footer.vue'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    HighOrLow,
    Footer,
    Header
  }
}
</script>

<style>
:root {
    --clr-bg: hsl(210, 10%, 80%);
    --clr-bg-hf: 	hsl(210, 10%, 23%);
    --clr-bg-hf-secondary: hsl(210, 10%, 35%);
    --clr-bg-hf-btn: hsl(210, 10%, 45%);
    --clr-text: azure;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--clr-text)
}
</style>
