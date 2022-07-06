<template>
  <div class="dashboard">
    <div class="title-container">
      <h1 class="title-item">Visualize seus Gastos</h1>
      <span v-if="total != null" class="title-item" >Total de Gastos: {{ total }}</span>
      <router-link to="/gastos" class="btn title-item">Cadastrar novo Gasto</router-link>
    </div>
    <div class="ver-gasto">
        <form @submit="getGasto($event)" class="gasto-form">
            <div class="input-box">
                <div class="input-container">
                    <label for="mes">Mês:</label>
                    <select name="mes" id="mes" v-model="mes">
                        <option value="1">Janeiro</option>
                        <option value="2">Fevereiro</option>
                        <option value="3">Março</option>
                        <option value="4">Abril</option>
                        <option value="5">Maio</option>
                        <option value="6">Junho</option>
                        <option value="7">Julho</option>
                        <option value="8">Agosto</option>
                        <option value="9">Setembro</option>
                        <option value="10">Outubro</option>
                        <option value="11">Novembro</option>
                        <option value="12">Dezembro</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="ano">Ano:</label>
                    <select name="ano" id="ano" v-model="ano">
                        <option :value="anoAtual">{{ anoAtual}}</option>
                        <option :value="anoAtual + 1">{{ anoAtual + 1}}</option>
                        <option :value="anoAtual + 2">{{ anoAtual + 2}}</option>
                        <option :value="anoAtual + 3">{{ anoAtual + 3}}</option>
                    </select>
                </div>
            </div>
            <InputSubmit text="Selecionar" />
        </form>
    </div>
    <TabelaGastos v-if="gastos.length > 0" :gastos="gastos"/>
    <div v-if="msg != null">
        {{msg}}
    </div>
  </div>
</template>

<script>
import InputSubmit from '@/components/form/InputSubmit.vue'
import TabelaGastos from '@/components/TabelaGastos.vue'

export default {
  components: { InputSubmit, TabelaGastos },
    name: 'Home',
    data() {
        return {
            gastos: {},
            total: null,
            mes: null,
            ano: null,
            anoAtual: new Date().getFullYear(),
            msg: null
        }
    },
    methods: {
        async getGasto(e) {
            e.preventDefault()

            const mes = this.mes
            const ano = this.ano

            // pegar token do usuario
            const token = this.$store.getters.token

            await fetch('http://localhost:3000/api/gasto/mes', {
                method: 'GET',
                headers: {
                    'Content-type': 'application/json',
                    'auth-token': token,
                    'mes': mes,
                    'ano': ano
                }
            })
            .then((resp) => resp.json())
            .then((data) => {
                this.gastos = data.gastos
                if(data.gastos.length === 0) {
                    this.msg = 'Nenhum gasto encontrado nesta data!'
                } else {
                    this.msg = null
                }
            })
            .catch((err) => console.log(err))
            // APAGAR ANTES DE FINALIZAR
            this.getTotal()
        },
        getTotal() {
            
            const gastos = this.gastos
            let total = 0
            gastos.forEach((gasto) => {
                let preco = gasto.preco.slice(2)
                preco = preco.replace('.', '').replace(',', '.')
                total += Number(preco)
            })

            const valor = total.toLocaleString('pt-br', {style: 'currency', currency: 'BRL'})
            const precoTotal = valor.toLocaleString('pt-br', {minimumFractionDigits: 2})

            this.total = precoTotal
        },
        getGastos() {
            const e = { preventDefault: function() {}}
            this.getGasto(e)
        }
    }
}
</script>

<style scoped>
.dashboard {
    padding: 50px;
    padding-bottom: 100px;
  }

  .title-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 30px;
  }

  .gasto-form {
        max-width: 400px;
        margin: 0 auto;
        display: flex;
        flex-direction: column;
        margin-bottom: 40px;
    }

    .input-box {
        display: flex;
        flex-direction: row;
        justify-content: center;
        margin-bottom: 10px;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin: 0;
        align-items: center;
    }

  .input-container select,
  .input-container label {
      color: #555;
      background-color: #fff;
      font-size: 14px;
      margin: 5px;
      margin-top: 0;
      border-radius: 5px;
      padding: 10px 16px;
      text-align: center;
  }

  .input-container select {
      padding: 10px;
      border: 1px solid #e8e8e8;
  }

  .input-container select:focus {
      border: 2px solid aquamarine;
  }

  .input-container select option {
      color: #fff;
  }

  .title-item {
      padding: 0 5px;
      margin-bottom: 15px;
      text-align: center;
  }

  @media (max-width: 600px) {
      .title-container {
          flex-direction: column;
      }

      .dashboard {
          padding: 50px 10px;
          transition: .5s;
      }
      
  }

  @media (max-width: 360px) {
      .dashboard {
          font-size: 0.7rem;
      }
  }

  @media (max-width: 260px) {
      .dashboard {
          font-size: 0.4rem;
      }

      .input-container label,
      .input-container select {
          font-size: 0.5rem;
          padding: 3px;
      }

      .gasto-form {
          width: 130px;
      }

      input[type="submit"] {
          width: 100%;
          margin: 0;
      }
  }
</style>