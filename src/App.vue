<template>
  <div>
    <div class="conteudo">
      <Navbar />
      <Sidebar />
      <router-view class="container"></router-view>
    </div>
    <Footer />
  </div>
</template>

<script>
import Navbar from './components/Navbar'
import Footer from './components/Footer.vue'
import Sidebar from './components/Sidebar.vue'

export default {
  name: "App",
  components: { Navbar, Footer, Sidebar }
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
    display: grid;
    grid-template-columns: auto 1fr;
    grid-template-rows: auto 1fr;
    grid-template-areas: 
      'sidebar navbar'
      'sidebar conteudo';
      
  }

  .container {
    min-height: 300px;
    grid-area: conteudo;
  }

  a {
    transition: .5s;
  }

  a:hover {
    color: #C1B696;
  }
</style>
