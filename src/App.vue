<template>
  <div>
    <div class="conteudo">
      <Navbar />
      <router-view class="container"></router-view>
    </div>
    <Footer />
  </div>
</template>

<script>
import Navbar from './components/Navbar'
import Footer from './components/Footer.vue'

export default {
  name: "App",
  components: { Navbar, Footer }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Helvetica;
    color: #444;
  }

  .conteudo {
    height: auto;   
    min-height: 100vh;
  }

  .container {
    min-height: 300px;
  }

  a {
    transition: .5s;
  }

  a:hover {
    color: #C1B696;
  }
</style>
