<template>
  <div>
    <ul v-if="totalPaginas > 1">
      <li v-for="pagina in paginas" :key="pagina">
        <router-link :to="{query: query(pagina)}">{{pagina}}</router-link>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "PaginacaoJogos",
  props: {
    jogosPorPagina: {
      type: Number,
      default: 1,
    },
    totalJogos: {
      type: Number,
      default: 1,
    },
  },
  methods: {
    query(pagina) {
      return {
        ...this.$route.query,
        _page: pagina,
      };
    },
  },
  computed: {
    paginas() {
      const paginaAtual = Number(this.$route.query._page);
      const paginasMaximas = 3;
      const offset = Math.ceil(paginasMaximas / 2);
      const total = this.totalPaginas;
      const paginasArray = [];

      for (let i = 1; i <= total; i++) {
        paginasArray.push(i);
      }

      paginasArray.splice(0, paginaAtual - offset);
      paginasArray.splice(paginasMaximas, total);
      return paginasArray;
    },
    totalPaginas() {
      const total = this.totalJogos / this.jogosPorPagina;
      return total !== Infinity ? Math.ceil(total) : 0;
    },
  },
};
</script>

<style scoped>
ul {
  grid-column: 1 / -1;
}

li {
  display: inline-block;
}

li a {
  padding: 2px 8px;
  border-radius: 2px;
  margin: 4px;
  font-weight: bold;
}

li a.router-link-exact-active,
li a:hover {
  background: rgb(219, 240, 26);
  color: #303030;
}
</style>