<template>
  <form class="adicionar-produto">
    <label for="nome">Nome</label>
    <input type="text" id="nome" name="nome" v-model="produto.nome" />
    <label for="preco">Preço (R$)</label>
    <input type="number" id="preco" name="preco" v-model="produto.preco" />
    <label for="fotos">Fotos</label>
    <label class="file-fotos">
      Selecione um arquivo...
      <input id="fotos" name="fotos" type="file" multiple ref="fotos" />
    </label>
    <label for="descricao">Descrição</label>
    <textarea id="descricao" name="descricao" v-model="produto.descricao"></textarea>
    <input class="btn" type="butto" value="Adicionar Produto" @click.prevent="adicionarProduto" />
  </form>
</template>

<script>
import { api } from "@/services.js";
export default {
  name: "AdicionarProduto",
  data() {
    return {
      produto: {
        nome: "",
        preco: "",
        descricao: "",
        fotos: null,
        vendido: "false",
      },
    };
  },
  methods: {
    formatarProduto() {
      const form = new FormData();
      const files = this.$refs.fotos.files;
      console.log(files);
      for (let i = 0; i < files.length; i++) {
        form.append(files[i].name, files[i]);
      }

      form.append("nome", this.produto.nome);
      form.append("preco", this.produto.preco);
      form.append("descricao", this.produto.descricao);
      form.append("vendido", this.produto.vendido);
      form.append("usuario_id", this.$store.state.usuario.id);

      return form;
    },
    adicionarProduto() {
      const produto = this.formatarProduto();
      api.post("/produto", produto).then(() => {
        this.$store.dispatch("getUsuarioProdutos");
      });
    },
  },
};
</script>

<style scoped>
.adicionar-produto {
  display: grid;
  grid-template-columns: 100px 1fr;
  align-items: center;
  margin-bottom: 60px;
}

.btn {
  grid-column: 2;
}

input[type="file"] {
  display: none;
}

.file-fotos {
  background-color: #fff;
  border-radius: 4px;
  border: 1px solid white;
  padding: 15px;
  box-shadow: 0 4px 8px rgba(30, 60, 90, 0.1);
  color: #858585;
  cursor: pointer;
  margin-bottom: 10px;
}
</style>