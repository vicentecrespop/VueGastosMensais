<template>
    <div class="dashboard">
        <div class="title-container">
            <h1 class="title-item">Histórico de Gastos</h1>
            <router-link to="/gastos" class="btn title-item">Adicionar Gasto</router-link>
        </div>
        <div v-if="gastos.length > 0">
            <TabelaGastos :gastos="gastos" />
        </div>
        <div v-else>
            <p>Você ainda não tem festas cadastradas, <router-link to="/gastos">clique aqui para adicionar um gasto!</router-link></p>
        </div>
    </div> 
</template>

<script>
import TabelaGastos from '../components/TabelaGastos.vue'

export default {
    name: 'Historico',
    components: { TabelaGastos },
    data() {
        return {
            gastos: {}
        }
    },  
    created() {
        this.getGastos()
    },
    methods: {
        async getGastos() {
            // pegar token do usuario
            const token = this.$store.getters.token

            await fetch('https://node-gastos-mensais.vercel.app/api/gasto', {
                method: 'GET',
                headers: {
                    'Content-type': 'application/json',
                    'auth-token': token
                }
            })
            .then((resp) => resp.json())
            .then((data) => {
                this.gastos = data.gastos
                this.editarDados()
            })
            .catch((err) => console.log(err))

        },
        editarDados() {

            this.gastos.forEach(gasto => {
                const data = String(gasto.data).slice(0, 10)
                gasto.data = data.split('-').reverse().join('/')

                const valor = gasto.preco.toLocaleString('pt-br', {style: 'currency', currency: 'BRL'})
                gasto.preco = valor.toLocaleString('pt-br', {minimumFractionDigits: 2})
            })
        }
    }
}
</script>

<style scoped>

 .dashboard {
    padding: 50px;
    padding-bottom: 100px;
    transition: .5s;
  }

  .title-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 40px;
  }

  .title-item {
      margin: 10px;
      text-align: center;
  }

  .btn {
    padding: 10px 16px;
    background-color: #000;
    color: #FFF;
    margin: 5px;
    text-decoration: none;
    border: none;
    cursor: pointer;
    font-size: 14px;
    transition: .5s;
  }

  .btn:hover {
    background-color: #141619;
  }

  @media (max-width: 575px) {
      .dashboard {
          padding: 50px 10px;
      }
  }

  @media (max-width: 500px) {
      .title-container {
          font-size: 0.7rem;
      }
  }

  @media (max-width: 395px) {
      .title-container {
          flex-direction: column;
      }
  }

  @media (max-width: 160px) {
      .title-container {
          font-size: 0.4rem;
      }

      .btn {
          font-size: 0.4rem;
      }
  }
</style>