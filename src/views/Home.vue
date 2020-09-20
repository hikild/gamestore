<template>
  <section>
    <div class="banner-titulo">
      <h1>Compre ou Venda</h1>
      <img src="@/assets/banner.svg" alt="Banner" />
    </div>
    <ProdutosBusca />
    <ProdutosLista />
  </section>
</template>

<script>
import ProdutosBusca from "@/components/ProdutosBusca.vue";
import ProdutosLista from "@/components/ProdutosLista.vue";

export default {
  name: "Home",
  components: {
    ProdutosBusca,
    ProdutosLista,
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap");

h1 {
  color: #ffffff;
  margin-top: 40px;
  font-size: 4rem;
  text-align: center;
  font-weight: normal;
  font-family: "Bebas Neue", cursive;
  padding: 11px;
  box-shadow: 11px 11px 0px 0px #dbf01a;
  text-decoration: underline;
  text-decoration-color: #f521a4;
}

.banner-titulo {
  display: flex;
  justify-content: center;
  align-items: center;
}

.banner-titulo img {
  width: 700px;
  padding: 60px;
}
</style>