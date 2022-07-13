<template>
    <div id="sidebar" style="margin-left: -150px;">
        <div class="sidebar-links">
          <router-link to="/" v-show="authenticated">Gastos</router-link>
          <router-link to="/login" v-show="!authenticated">Entrar</router-link>
          <router-link to="/register" v-show="!authenticated">Cadastrar</router-link>
          <router-link to="/historico" v-show="authenticated">Histórico</router-link>
          <router-link to="/profile" v-show="authenticated">Configurações</router-link>
          <button @click="logout($event)" v-show="authenticated">Logout</button>
        </div>
        <div class="sidebar-button" @click="mostrarLinks">
            <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" fill="currentColor" class="bi bi-justify" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M2 12.5a.5.5 0 0 1 .5-.5h11a.5.5 0 0 1 0 1h-11a.5.5 0 0 1-.5-.5zm0-3a.5.5 0 0 1 .5-.5h11a.5.5 0 0 1 0 1h-11a.5.5 0 0 1-.5-.5zm0-3a.5.5 0 0 1 .5-.5h11a.5.5 0 0 1 0 1h-11a.5.5 0 0 1-.5-.5zm0-3a.5.5 0 0 1 .5-.5h11a.5.5 0 0 1 0 1h-11a.5.5 0 0 1-.5-.5z"/>
            </svg>
        </div>
    </div>
</template>

<script>
import { mapState } from 'vuex'

export default {
    name: 'Sidebar',
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
            sidebar.style.marginLeft = sidebar.style.marginLeft == '-150px' ? '0px' : '-150px'
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
    #sidebar {
        height: 100%;
        width: 150px;
        grid-area: sidebar;
        transition: 1s;
        display: flex;
    }

    .sidebar-links {
        width: 150px;
        display: flex;
        flex-direction: column;
        height: 100%;
        background-color: black;
        color: #fff;
        padding-right: 40px;
    }

    .sidebar-button {
        width: 60px;
        padding-top: 25px;
        margin-left: 40px;
    }

    .sidebar-button svg:hover {
        border: 1px solid black;
    }

    .sidebar-button svg:active {
        padding: 3px;
    }

    .sidebar-links button {
        background-color: black;
        border: 0;
        font-size: 1rem;
    }

    .sidebar-links a,
    .sidebar-links button {
        text-decoration: none;
        width: 110px;
        padding: 10px;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        color: rgb(94, 88, 88);
    }

    .sidebar-links a:hover,
    .sidebar-links button:hover {
        color: #aaa;
    }

    @media(max-width: 460px) {
        .sidebar-button {
            margin-left: 10px;
        }
    }

</style>