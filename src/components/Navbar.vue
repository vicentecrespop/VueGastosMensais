<template>
  <div id="nav">
      <!-- <router-link to="/" id="logo-container">
          <img src="/img/partytimelogo.png" alt="Party time">
      </router-link> -->
      <div class="nav-title">
          <h2>Gastos Mensais</h2>
      </div>
      <div class="nav-links">
          <router-link to="/" v-show="authenticated">Gastos</router-link>
          <router-link to="/login" v-show="!authenticated">Entrar</router-link>
          <router-link to="/register" v-show="!authenticated">Cadastrar</router-link>
          <router-link to="/historico" v-show="authenticated">Histórico</router-link>
          <router-link to="/profile" v-show="authenticated">Configurações</router-link>
          <button @click="logout($event)" v-show="authenticated">Logout</button>
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
        align-items: center;
        justify-content: space-between;
        padding: 20px;
        border-bottom: 1px solid #EEE;
        height: 80px;
    }

    #nav a {
        text-decoration: none;
        padding-right: 15px;
    }

    #nav img {
        width: 35px;
    }

    .nav-title {
        font-weight: 300;
        text-align: center;
        /* width: 30%; */
    }

    /* #logo-container, .nav-links {
        width: 35%;
    } */

    .nav-links {
        display: flex;
        justify-content: flex-end;
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

    @media (max-width: 550px) {
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
    }

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