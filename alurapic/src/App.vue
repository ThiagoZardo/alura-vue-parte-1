<template>
  <div>
    <h1>{{ titulo }}</h1>
    <ul>
      <li v-for="foto of fotos">
        <img :src="foto.url" :alt="foto.titulo" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      titulo: "AluraPic",
      fotos: [ ]
    };
  },
  // As funções dentro de created serão executadas no carregamento da página.
  created() {
    this.$http.get('http://localhost:3000/v1/fotos')
      .then(res => res.json())
      .then(foto => this.fotos = foto, err => console.log(err));
  }
};
</script>

<style>
</style>
