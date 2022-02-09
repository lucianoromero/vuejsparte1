<template>
    <img class="imagem-responsiva" :src="url" :alt="titulo">
</template>

<script>

/*Propriedades que o Elemento pode receber */
export default {
    props: ["url", "titulo"],
}

</script>

<style scoped>
/* estilo imagem */
    .imagem-responsiva {
        width: 100%;
    }
</style>