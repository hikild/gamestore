<template>
  <section>
    <div v-if="vendas">
      <h2>Suas vendas</h2>
      <div class="produtos-wrapper" v-for="(venda, index) in vendas" :key="index">
        <ProdutoItem v-if="venda.produto" :produto="venda.produto">
          <p class="vendedor">
            <span>Comprador:</span>
            {{venda.comprador_id}}
          </p>
        </ProdutoItem>
        <div class="entrega">
          <h3>Entrega:</h3>
          <ul v-if="venda.endereco">
            <li v-for="(valor, chave) in venda.endereco" :key="chave">{{chave}}: {{valor}}</li>
          </ul>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import ProdutoItem from "@/components/ProdutoItem.vue";
import { api } from "@/services.js";
import { mapState } from "vuex";
export default {
  name: "UsuarioVendas",
  data() {
    return {
      vendas: null,
    };
  },
  computed: {
    ...mapState(["usuario", "login"]),
  },
  components: {
    ProdutoItem,
  },
  methods: {
    getVendas() {
      api.get("/transacao?tipo=vendedor_id").then((r) => {
        this.vendas = r.data;
      });
    },
  },
  watch: {
    login() {
      this.getVendas();
    },
  },
  created() {
    if (this.login) {
      this.getVendas();
    }
  },
};
</script>

<style scoped>
.produto-wrapper {
  margin-bottom: 40px;
}
.vendedor span {
  color: #e83766;
}
h2 {
  margin-bottom: 20px;
}

.entrega {
  display: grid;
  grid-template-columns: minmax(100px, 200px) 1fr;
  grid-gap: 20px;
  margin-bottom: 60px;
}

h3 {
  margin: 0;
  justify-self: end;
}
</style>