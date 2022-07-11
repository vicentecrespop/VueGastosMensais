<template>
    <div id="sidebar">
        <div class="sidebar-links">
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
            const links = document.getElementById('sidebar-links')
            links.style.display = links.style.display == 'flex' ? 'none' : 'flex'
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
        color: #fff;
        background-color: black;
        grid-area: sidebar;
        transition: 1s;
        margin-left: -150px;
    }

    .sidebar-links {
        display: flex;
        flex-direction: column;
        height: 100%;
    }
    .sidebar-links button {
        background-color: black;
        border: 0;
        font-size: 1rem;
        position: relative;
        bottom: 10px;
    }

    .sidebar-links a,
    .sidebar-links button {
        text-decoration: none;
        padding: 10px;
        display: flex;
        justify-content: flex-start;
        align-items: center;
    }

</style>