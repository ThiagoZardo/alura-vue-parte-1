
<template>
  <div>
    <h1 class="centralizado">{{ titulo }}</h1>
    <input type="search" class="filtro" @input="filtro = $event.target.value" placeholder="filtre por parte do título">

    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComFiltro" v-bind:key="foto">
        <meu-painel :titulo="foto.titulo">
            <imagem-responsiva :url="foto.url" :titulo="foto.titulo"/>
        </meu-painel>
      </li>
    </ul>
  </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue';
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue';

export default {

  components: {
    'meu-painel' : Painel, 
    'imagem-responsiva': ImagemResponsiva
  },

  data() {

    return {
      titulo: 'Alurapic', 
      fotos: [], 
      filtro: ''
    }
  },

  computed: {

    fotosComFiltro() {

      if(this.filtro) {
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }
    }
  },

  created() {

    this.$http.get('http://localhost:3000/v1/fotos')
      .then(res => res.json())
      .then(fotos => this.fotos = fotos, err => console.log(err));
  }
}

</script>

<style>

  .centralizado {
    text-align: center;
  }

  .lista-fotos {
    list-style: none;
  }

  .lista-fotos .lista-fotos-item {
    display: inline-block;
  }

  .filtro {
    font-size: 24px;
    display: block;
    width: 100%;
  }

  .pagina-enter,
  .pagina-leave-active {
    opacity: 0;
  }

  .pagina-enter-active,
  .pagina-leave-active {
    transition: opacity .3s;
  }
</style>
