<template>
  <div class="main">
    <div class="conteudo">
      <Navbar />
      <Sidebar />
      <router-view class="containers"></router-view>
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

  .main {
    height: auto;
    min-height: 100vh;
    width: 100vw;
  }

  .conteudo {
    height: auto;   
    min-height: 100vh;
    width: 100vw;
    display: grid;
    grid-template-columns: auto 1fr;
    grid-template-rows: auto 1fr;
    grid-template-areas: 
      'sidebar navbar'
      'sidebar conteudo';
      
  }

  .containers {
    min-height: 300px;
    grid-area: conteudo;
    margin-top: 80px;
    width: 100%;
  }

  a {
    transition: .5s;
  }

  a:hover {
    color: #C1B696;
  }
</style>
