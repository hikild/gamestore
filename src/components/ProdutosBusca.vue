<template>
  <form>
    <input name="busca" id="busca" type="text" v-model="busca" placeholder="Buscar por itens..." />
    <input type="submit" id="lupa" value="buscar" @click.prevent="buscarJogos" />
    {{busca}}
  </form>
</template>

<script>
export default {
  name: "ProdutosBusca",
  data() {
    return {
      busca: "",
    };
  },
  methods: {
    buscarJogos() {
      this.$router.push({ query: { q: this.busca } });
    },
  },
};
</script>

<style scoped>
form {
  max-width: 600px;
  margin: 30px auto 60px auto;
  position: relative;
}

#busca {
  width: 100%;
  padding: 20px;
  border: none;
}

#busca:focus,
#busca:hover {
  transform: scale(1.1);
}

#lupa {
  width: 62px;
  height: 62px;
  background: url("../assets/search.svg") no-repeat center center;
  text-indent: -150px;
  border: none;
  cursor: pointer;
  position: absolute;
  top: 0px;
  right: 0px;
  box-shadow: none;
}
</style>