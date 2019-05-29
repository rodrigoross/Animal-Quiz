<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">{{ current.question }}</template>

      <hr class="my-4">

      <p>Lista de opcoes</p>

    <b-button variant="primary" href="#">Enviar</b-button>
        <b-button @click="next" variant="success" href="#">
            Proxima questao
        </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    current: Object,
    next: Function
  }
};
</script>

<style lang="scss" scoped>
</style>