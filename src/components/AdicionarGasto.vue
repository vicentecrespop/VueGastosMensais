<template>
  <div class="add-gasto">
      <Message :msg="msg" :msgClass="msgClass" />
      <form id="gasto-form" @submit="addGasto($event)">
        <div class="input-container">
            <label for="nome">Gasto:</label>
            <input type="text" name="nome" id="nome" v-model="nome" placeholder="Qual é o gasto?">
        </div>
        <div class="input-container">
            <label for="preco">Preço:</label>
            <input type="number" step="0.01" name="preco" id="preco" v-model="preco" placeholder="Preço do gasto">
        </div>
        <div class="input-container">
            <label for="tipo">Tipo de Gasto:</label>
            <select name="tipo" id="tipo" v-model="tipo">
                <option value="Alimentação">Alimentação</option>
                <option value="Entretenimento">Entretenimento</option>
                <option value="Moradia">Moradia</option>
                <option value="Roupas">Roupas</option>
                <option value="Transporte">Transporte</option>
                <option value="Outros">Outros</option>
            </select>
        </div>
        <div class="input-container">
            <label for="data">Data:</label>
            <input type="date" name="data" id="data" v-model="data">
        </div>
        <input-submit text="Adicionar" />
      </form>
  </div>
</template>

<script>
import InputSubmit from './form/InputSubmit.vue'
import Message from './Message.vue'

export default {
  components: { InputSubmit, Message },
    name: 'AdicionarGastos',
    data() {
        return {
            nome: null,
            tipo: null,
            preco: null,
            data: null,
            msg: null,
            msgClass: null
        }
    },
    methods: {
        async addGasto(e) {
            e.preventDefault()

            const gasto = {
                nome: this.nome,
                preco: this.preco,
                tipo: this.tipo,
                data: this.data
            }

            const jsonData = JSON.stringify(gasto)

            // pegar token
            const token = this.$store.getters.token

            await fetch('https://nodegastosmensais.herokuapp.com/api/gasto', {
                method: 'POST',
                headers: {
                    'Content-type': 'application/json',
                    'auth-token': token
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
                }

                setTimeout(() => {
                    if(!data.error) {
                        this.msg = null
                        this.$router.push('/historico')
                    }
                }, 2000)
            })
            .catch((err) => console.log(err)) 
            // APAGAR ANTES DE FINALIZAR


        }
    }
}
</script>

<style scoped>
    #gasto-form {
        width: 60%;
        max-width: 400px;
        min-width: 200px;
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

    .input-container input,
    .input-container select {
        padding: 10px;
        border: 1px solid #e8e8e8;
    }

    .input-container input:focus,
    .input-container select:focus {
        outline: 0;
        border: 2px solid #25282E;
        box-shadow: 0 0 0 0 ;
    }

    .input-container select {
        background-color: #fff;
    }
</style>