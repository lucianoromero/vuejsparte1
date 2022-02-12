<template>
  <div>
    <h1 class="centralizado">{{ titulo }}</h1>

    <input
      type="search"
      class="filtro"
      v-on:input="filtro = $event.target.value"
      placeholder="filtre pelo título da foto"
    />

    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComFiltro" :key="foto.titulo">
        <meu-painel :titulo="foto.titulo">
          <!-- <img class="imagem-responsiva" :src="foto.url" :alt="foto.titulo" /> -->
          <imagem-responsiva :url="foto.url" :titulo="foto.titulo" />
          <meu-botao rotulo="remover" tipo="button" @botaoAtivado="remove(foto)"/>
        </meu-painel>
 
      </li>
    </ul>
  </div>
</template>

<script>
import Painel from "../shared/painel/Painel.vue";
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue'
import Botao from '../shared/botao/Botao.vue';

export default {

  components: {
    "meu-painel": Painel,
    "imagem-responsiva": ImagemResponsiva,
    'meu-botao': Botao,
  },

  methods: {

    remove(foto) {
        alert('Precisa saber qual foto remover!' + foto.titulo);
    }
  },

  data() {
    return {
      titulo: "Alurapic",
      fotos: [],
      filtro: "",
    };
  },
  //Sempre que tivemos que realizar algum cálculo ou aplicar alguma lógica dinamicamente podemos usar computed property
  computed: {
    fotosComFiltro() {
      if (this.filtro) {
        // criando uma expressão com o valor do filtro, insensitivo
        let exp = new RegExp(this.filtro.trim(), "i");
        // retorna apenas as fotos que condizem com a expressão
        return this.fotos.filter((foto) => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }
    },
  },

  created() {
    /*Realizando a requisação para API e fazendo o bind para a lista*/
    this.$http
      .get("http://localhost:3000/v1/fotos")
      .then((res) => res.json())
      .then(
        (fotos) => (this.fotos = fotos),
        (err) => console.log(err)
      );
  },
};
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
  display: block;
  width: 100%;
}
</style>
