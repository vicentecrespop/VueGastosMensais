<template>
  <div>
      <Message :msg="msg" :msgClass="msgClass" />
      <form @submit="page === 'register' ? register($event) : update($event)" id="user-form">
          <input type="hidden" name="id" id="id" v-model="id">
          <div class="input-container">
              <label for="name">Nome:</label>
              <input type="text" id="name" name="name" v-model="name" placeholder="Digite o seu nome.">
          </div>
          <div class="input-container">
              <label for="email">E-mail:</label>
              <input type="text" id="email" name="email" v-model="email" placeholder="Digite o seu email.">
          </div>
          <div class="input-container">
              <label for="password">Senha:</label>
              <input type="password" id="password" name="password" v-model="password" placeholder="Digite a sua senha.">
          </div>
          <div class="input-container">
              <label for="confirmpassword">Confirmação de senha:</label>
              <input type="password" id="confirmpassword" name="confirmpassword" v-model="confirmpassword" placeholder="Confirme a sua senha.">
          </div>
          <InputSubmit :text="btnText"/>
      </form>
  </div>
</template>

<script>
import InputSubmit from "./form/InputSubmit.vue";
import Message from './Message.vue'

export default {
    name: "RegisterForm",
    data() {
        return {
            id: this.user._id || null,
            name: this.user.name || null,
            email: this.user.email || null,
            password: null,
            confirmpassword: null,
            msg: null,
            msgClass: null
        };
    },
    props: ['user', 'page', 'btnText'],
    components: { InputSubmit, Message },
    methods: {
        async register(e) {
            e.preventDefault()

            const data = {
                name: this.name,
                email: this.email,
                password: this.password,
                confirmpassword: this.confirmpassword
            }

            const jsonData = JSON.stringify(data)

            await fetch("http://localhost:3000/api/auth/register", {
                method: "POST",
                headers: { "Content-type": "application/json" },
                body: jsonData
            })
            .then((resp) => resp.json())
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
            .catch((err) => {
                console.log(err)
            })
        },
        async update(e) {
            e.preventDefault()
            // console.log('Editando...')

            const data = {
                id: this.id,
                name: this.name,
                email: this.email,
                password: this.password,
                confirmpassword: this.confirmpassword
            }

            const jsonData = JSON.stringify(data)

            // pegar token da store
            const token = this.$store.getters.token

            await fetch("https://nodegastosmensais.herokuapp.com/api/user", {
                method: "PATCH",
                headers: { 
                    "Content-type": "application/json",
                    "auth-token": token
                },
                body: jsonData
            })
            .then((resp) => resp.json())
            .then((data) => {

                if(data.error) {
                    this.msg = data.error
                    this.msgClass = 'error'
                } else {
                    this.msg = data.msg
                    this.msgClass = 'success'
                    setTimeout(() => {
                        
                        this.msg = null
                        this.$router.push('/')
                            
                    }, 2000)
                }
                
            })
            .catch((err) => {
                console.log(err)
            })

        }
    }
}
</script>

<style scoped>
    #user-form {
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