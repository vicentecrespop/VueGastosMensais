<template>
    <div class="data-container">
        <div class="data-table-heading">
            <div class="data-id-heading">Nª:</div>
            <div class="data-title-heading">Nome do Gasto:</div>
            <div class="data-price-heading">Preço:</div>
            <div class="data-type-heading">Tipo:</div>
            <div class="data-date-heading">Data:</div>
            <div class="data-actions-heading">Ações:</div>
        </div>
        <div class="data-table body">
            <div class="data-row" v-for="(gasto, index) in gastos" :key="index">
                <div class="data-id-container">{{ index + 1}}</div>
                <div class="data-title-container">
                    {{ gasto.nome }}
                </div>
                <div class="data-price-container">
                    {{ gasto.preco }}
                </div>
                <div class="data-type-container">
                    {{ gasto.tipo }}
                </div>
                <div class="data-date-container">
                    {{ String(gasto.data).slice(0, 10) }}
                </div>
                <div class="data-actions-container">
                    <button class="remove-btn" @click="remove(gasto._id)">Remover</button>
                </div>
            </div>
        </div>
    </div>  
</template>

<script>
export default {
    name: 'TabelaGastos',
    props: ['gastos'],
    created() {
        this.editarDados()
    },
    methods: {
        async remove(id) {
            // pegar token do usuario
            const token = this.$store.getters.token

            const data = { id }

            const jsonData = JSON.stringify(data)

            await fetch('http://localhost:3000/api/gasto', {
                method: 'DELETE',
                headers: {
                    'Content-type': 'application/json',
                    'auth-token': token
                },
                body: jsonData
            })
            .then(() => {
                console.log('Gasto removido com sucesso!')
                setTimeout(() => {
                     this.msg = null

                     // carregar todas festas denovo
                     this.$parent.getGastos()
                 }, 500)
            })
            .catch((err) => console.log(err))
            // APAGAR ANTES DE FINALIZAR


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
    .data-table-heading, .data-row {
        display: flex;
        align-items: center;
        height: 50px;
        border-bottom: 1px solid #CCC;
    }

    .data-table-heading div, .data-row div {
        flex: 1 1 0;
    }

    .data-id-heading, .data-id-container {
        max-width: 50px;
    }

    .edit-btn, .remove-btn {
        padding: 10px 16px;
        background-color: #000;
        color: #FFF;
        margin: 5px;
        text-decoration: none;
        border: none;
        font-size: 14px;
        cursor: pointer;
        transition: .5s;
    }

    .edit-btn {
        background-color: #007BFF;
    }

    .edit-btn:hover {
        background-color: #0069D9;
    }

    .remove-btn {
        background-color: #DC3545;
    }

    .remove-btn:hover {
        background-color: #C82333;
    }

    @media (max-width: 700px) {
      .data-container,
      .remove-btn {
          font-size: 0.6rem;
      }

    }

    @media (max-width: 420px) {
        .data-container,
        .remove-btn {
            font-size: 0.3rem;
        }

    }

    @media (max-width: 280px) {
        .data-container {
            width: 230px;
            margin: auto;
        }
    }
</style>