<template>
    <footer>
        <h3>Gastos Mensais &copy; 2022</h3>
        <p>Criado por Vicente Crespo</p>
    </footer>
</template>

<script>
export default {
    name: "Footer"
}
</script>

<style scoped>
    footer {
        background-color: #25282E;
        height: 150px;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
    }

    footer h3 {
        color: #FFF;
        margin-bottom: 15px;
        font-weight: 300;
        font-size: 1.6rem;
    }

    footer p {
        color: #C1B696;
    }
</style>