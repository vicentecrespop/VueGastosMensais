<template>
  <div class="register">
      <h1>Criar uma conta</h1>
      <UserForm :user="{}" page="register" btnText="Cadastrar"/>
  </div>
</template>

<script>
import UserForm from '@/components/UserForm.vue';
export default {
    name: "Register",
    components: { UserForm },
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
    .register {
        text-align: center;
        padding-top: 40px;
        padding-bottom: 100px;
    }

    .register h1 {
        margin-bottom: 40px;
        transition: .5s;
    }

    @media (max-width: 270px) {
        .register h1 {
            font-size: 1.3rem;
        }
    }
</style>