<template>
  <div id="nav">
      <div id="nav-button" class="nav-button" @click="mostrarLinks">
            <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" fill="currentColor" class="bi bi-justify" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M2 12.5a.5.5 0 0 1 .5-.5h11a.5.5 0 0 1 0 1h-11a.5.5 0 0 1-.5-.5zm0-3a.5.5 0 0 1 .5-.5h11a.5.5 0 0 1 0 1h-11a.5.5 0 0 1-.5-.5zm0-3a.5.5 0 0 1 .5-.5h11a.5.5 0 0 1 0 1h-11a.5.5 0 0 1-.5-.5zm0-3a.5.5 0 0 1 .5-.5h11a.5.5 0 0 1 0 1h-11a.5.5 0 0 1-.5-.5z"/>
            </svg>
      </div>
          <div class="nav-title">
              <svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" fill="currentColor" class="bi bi-coin" viewBox="0 0 16 16">
                <path d="M5.5 9.511c.076.954.83 1.697 2.182 1.785V12h.6v-.709c1.4-.098 2.218-.846 2.218-1.932 0-.987-.626-1.496-1.745-1.76l-.473-.112V5.57c.6.068.982.396 1.074.85h1.052c-.076-.919-.864-1.638-2.126-1.716V4h-.6v.719c-1.195.117-2.01.836-2.01 1.853 0 .9.606 1.472 1.613 1.707l.397.098v2.034c-.615-.093-1.022-.43-1.114-.9H5.5zm2.177-2.166c-.59-.137-.91-.416-.91-.836 0-.47.345-.822.915-.925v1.76h-.005zm.692 1.193c.717.166 1.048.435 1.048.91 0 .542-.412.914-1.135.982V8.518l.087.02z"/>
                <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
                <path d="M8 13.5a5.5 5.5 0 1 1 0-11 5.5 5.5 0 0 1 0 11zm0 .5A6 6 0 1 0 8 2a6 6 0 0 0 0 12z"/>
              </svg>
              <h2>Gastos Mensais</h2>
          </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'

export default {
    name: "Navbar",
    methods: {
        logout(e) {
            e.preventDefault()

            // emitir evento de logout do usuario
            this.$store.commit('logout')

            // redirect
            this.$router.push('/login')
        },
        mostrarLinks() {
            const sidebar = document.getElementById('sidebar')
            const botao = document.getElementById('nav-button')
            // sidebar.style.left = sidebar.style.left == '0px' ? '-150px' : '0px'
            sidebar.style.marginLeft = sidebar.style.marginLeft == '-150px' ? '0px' : '-150px'
            botao.style.left = sidebar.style.marginLeft == '0px' ? '190px' : '40px'
        }
    },
    computed: {
        ...mapState([
            'authenticated'
        ])
    }
}
</script>

<style scoped>
    #nav {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 20px;
        border-bottom: 1px solid #EEE;
        height: 80px;
        transition: .5s;
        grid-area: navbar;
    }

    #nav a {
        text-decoration: none;
        padding-right: 15px;
    }

    .nav-title {
        font-weight: 300;
        font-size: 1.5rem;
        width: 100%;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        margin-bottom: 10px;
    }

    .nav-title svg {
        position: absolute;
        right: 20px;
        top: 15px;
    }


    .nav-button {
        position: absolute;
        left: 40px;
        top: 25px;
        transition: left 1s ease;
    }

    .nav-links {
        display: flex;
        justify-content: flex-end;
        display: none;
        transition: .5s;
        margin: auto 20px;
        width: 100%;
    }

    .nav-links a {
        text-decoration: none;
        width: 25%;
    }

    button {
        background-color: #FFF;
        border: none;
        font-size: 16px;
        cursor: pointer;
    }

    button:hover {
        color: #C1B696;
    }


    @media (max-width: 560px) {
        .nav-title svg {
            display: none;
        }

        .nav-title {
            justify-content: flex-start;
        }
    }

    /* @media (max-width: 550px) {
        #nav {
            flex-direction: column;
            height: 100px;
        }

        .nav-title {
            width: 250px;
        }


        .nav-links {
            margin-top: 5px;
        }
    } */

    @media (max-width: 330px) {

        #nav {
            height: 140px;
        }

        .nav-links {
            flex-wrap: wrap;
            width: 230px;
        }

        .nav-links a, .nav-links button {
            width: 50%;
            text-align: center;
            padding: 8px 5px;
        }
    }

    @media (max-width: 240px) {

        .nav-links {
            width: 170px;
        }
        .nav-links a, .nav-links button {
            font-size: 0.7rem;
        }


    }

</style>