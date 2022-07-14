<template>
    <div class="login">
        <h1>Entrar</h1>
        <LoginForm />
    </div>
</template>

<script>
import LoginForm from '@/components/LoginForm.vue';

export default {
    components: { LoginForm },
    created() {
        this.verificarLogado()
    },
    methods: {
        verificarLogado() {
            const auth = this.$store.getters.authenticated
            if(auth) {
                this.$router.push('/')
            }
        }
    }
}
</script>

<style scoped>
    .login {
        text-align: center;
        padding-top: 40px;
        padding-bottom: 100px;
    }

    .login h1 {
        margin: 40px 10px;
        text-align: center;
        transition: .5s;
    }

    @media (max-width: 250px) {
        .login h1 {
            font-size: 1.3rem;
        }
    }
</style>