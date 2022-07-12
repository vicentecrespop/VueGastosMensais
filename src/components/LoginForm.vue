<template>
  <div>
      <Message :msg="msg" :msgClass="msgClass" />
      <form @submit="login($event)" id="login-form">
        <div class="input-container">
            <label for="email">E-mail:</label>
            <input type="email" name="email" id="email" v-model="email" placeholder="Digite o seu e-mail">
        </div>
        <div class="input-container">
            <label for="password">Senha:</label>
            <input type="password" name="password" id="password" v-model="password" placeholder="Digite a sua senha">
        </div>
        <InputSubmit text="Entrar" />
      </form>
  </div>
</template>

<script>
import Message from './Message.vue'
import InputSubmit from './form/InputSubmit.vue'

export default {
    name: 'LoginForm',
    components: { Message, InputSubmit },
    data() {
        return {
            email: null,
            password: null,
            msg: null,
            msgClass: null
        }
    },
    methods: {
        async login(e) {
            e.preventDefault()

            const data = {
                email: this.email,
                password: this.password
            }

            const jsonData = JSON.stringify(data)

            await fetch('https://nodegastosmensais.herokuapp.com/api/auth/login', {
                method: 'POST',
                headers: { 'Content-type': 'application/json' },
                body: jsonData
            })
            .then((res) => res.json())
            .then((data) => {

                let auth = false

                if(data.error) {
                    this.msg = data.error
                    this.msgClass = 'error'
                } else {
                    auth = true
                    this.msg = data.msg
                    this.msgClass = 'success'

                    // emitir evento para autenticar usuario
                    this.$store.commit('authenticate', { token: data.token, userId: data.userId })
                }

                setTimeout(() => {
                    if(!auth) {
                        this.msg = null
                    } else {
                        // redirect
                        this.$router.push('/')
                    }
                }, 2000)
            })
        }
    }
}
</script>

<style scoped>
    #login-form {
        width: 60%;
        max-width: 400px;
        min-width: 100px;
        margin: 0 auto;
        display: flex;
        flex-direction: column;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 15px;
        text-align: left;
    }

    .input-container label {
        margin-bottom: 10px;
        color: #555;
    }

    .input-container input {
        padding: 10px;
        border: 1px solid #e8e8e8;
    }

    .input-container input:focus {
        outline: 0;
        border: 2px solid #25282E;
        box-shadow: 0 0 0 0 ;
    }

    input[type="submit"] {
        width: 100%;
        max-width: 200px;
    }
</style>