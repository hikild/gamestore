<template>
  <section class="produtos-container">
    <transition mode="out-in">
      <div v-if="produtos && produtos.length" class="produtos" key="jogos">
        <div class="produto" v-for="(produto, index) in produtos" :key="index">
          <router-link :to="{name: 'produto', params:{id:produto.id}}">
            <img v-if="produto.fotos" :src="produto.fotos[0].src" :alt="produto.fotos[0].titulo" />
            <h2 class="titulo">{{produto.nome}}</h2>
            <p class="preco">{{produto.preco | numeroPreco}}</p>
            <p class="descricao">{{produto.descricao}}</p>
          </router-link>
        </div>
        <PaginacaoJogos :totalJogos="totalJogos" :jogosPorPagina="jogosPorPagina" />
      </div>
      <div v-else-if="produtos && produtos.length === 0" key="sem-resultados">
        <p class="sem-resultados">Poxa! Não não há resultados nessa busca.</p>
      </div>
      <PaginaCarregando key="carregando" v-else />
    </transition>
  </section>
</template>

<script>
import PaginacaoJogos from "@/components/PaginacaoJogos.vue";
import { api } from "@/services.js";
import { serialize } from "@/helpers.js";

export default {
  name: "ProdutosLista",
  components: {
    PaginacaoJogos,
  },
  data() {
    return {
      produtos: null,
      jogosPorPagina: 6,
      totalJogos: 0,
    };
  },
  computed: {
    url() {
      const query = serialize(this.$route.query);
      return `/produto?_limit=${this.jogosPorPagina}${query}`;
    },
  },
  methods: {
    getProdutos() {
      this.produtos = null;
      setTimeout(() => {
        api.get(this.url).then((r) => {
          this.totalJogos = Number(r.headers["x-total-count"]);
          this.produtos = r.data;
        });
      }, 1500);
    },
  },
  watch: {
    url() {
      this.getProdutos();
    },
  },
  created() {
    this.getProdutos();
  },
};
</script>

<style scoped>
.produtos {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 30px;
  margin: 30px;
}

.produtos-container {
  max-width: 1000px;
  margin: 0 auto;
}

.produto {
  box-shadow: 0 4px 8px rgba(30, 60, 90, 0.1);
  padding: 10px;
  background: #ffff;
  border-radius: 4px;
  transition: all 0.2s;
}

.produto:hover {
  box-shadow: 0 6px 12px rgba(30, 60, 90, 0.2);
  transform: scale(1.1);
  position: relative;
  z-index: 1;
}

.produto img {
  border-radius: 4px;
  margin-bottom: 20px;
}

.titulo {
  margin-bottom: 10px;
}

.preco {
  font-weight: bold;
  color: #f521a4;
}

.sem-resultados {
  text-align: center;
}
</style>