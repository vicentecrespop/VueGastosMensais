<template>
  <div class="add-gasto">
      <h1>Adicionar Gastos Mensais</h1>
      <adicionar-gasto />
  </div>
</template>

<script>
import AdicionarGasto from '../components/AdicionarGasto.vue'

export default {
    name: 'Gasto',
    components: { AdicionarGasto }
}
</script>

<style scoped>
    .add-gasto h1 {
        text-align: center;
        padding: 10px;
        margin: 40px auto;
    }

    .add-gasto {
        padding-bottom: 40px;
    }
</style>