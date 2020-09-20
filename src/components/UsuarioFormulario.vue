<template>
  <form>
    <div class="usuario" v-if="mostrarDadosLogin">
      <label for="nome">Nome</label>
      <input type="text" name="nome" id="nome" v-model="nome" />
      <label for="email">Email</label>
      <input type="email" name="email" id="email" v-model="email" />
      <label for="senha">Senha</label>
      <input type="password" name="senha" id="senha" v-model="senha" />
    </div>
    <label for="cep">CEP</label>
    <input type="text" name="cep" id="cep" v-model="cep" @keyup="preencherCep" />
    <label for="rua">Rua</label>
    <input type="text" name="rua" id="rua" v-model="rua" />
    <label for="numero">Número</label>
    <input type="text" name="numero" id="numero" v-model="numero" />
    <label for="bairro">Bairro</label>
    <input type="text" name="bairro" id="bairro" v-model="bairro" />
    <label for="cidade">Cidade</label>
    <input type="text" name="cidade" id="cidade" v-model="cidade" />
    <label for="estado">Estado</label>
    <input type="text" name="estado" id="estado" v-model="estado" />
    <div class="button">
      <slot></slot>
    </div>
  </form>
</template>

<script>
import { getCep } from "@/services.js";
export default {
  name: "UsuarioFormulario",
  computed: {
    mostrarDadosLogin() {
      return !this.$store.state.login || this.$route.name === "editar";
    },
    nome: {
      get() {
        return this.$store.state.usuario.nome;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { nome: value });
      },
    },
    email: {
      get() {
        return this.$store.state.usuario.email;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { email: value });
      },
    },
    senha: {
      get() {
        return this.$store.state.usuario.senha;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { senha: value });
      },
    },
    cep: {
      get() {
        return this.$store.state.usuario.cep;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { cep: value });
      },
    },
    rua: {
      get() {
        return this.$store.state.usuario.rua;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { rua: value });
      },
    },
    numero: {
      get() {
        return this.$store.state.usuario.numero;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { numero: value });
      },
    },
    bairro: {
      get() {
        return this.$store.state.usuario.bairro;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { bairro: value });
      },
    },
    cidade: {
      get() {
        return this.$store.state.usuario.cidade;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { cidade: value });
      },
    },
    estado: {
      get() {
        return this.$store.state.usuario.estado;
      },
      set(value) {
        this.$store.commit("UPDATE_USUARIO", { estado: value });
      },
    },
  },
  methods: {
    preencherCep() {
      const cep = this.cep.replace(/\D/g, "");
      if (cep.length === 8) {
        getCep(cep).then((r) => {
          this.rua = r.data.logradouro;
          this.bairro = r.data.bairro;
          this.estado = r.data.uf;
          this.cidade = r.data.localidade;
        });
      }
    },
  },
};
</script>

<style scoped>
form,
.usuario {
  display: grid;
  grid-template-columns: 80px 1fr;
  align-items: center;
}

.usuario {
  grid-column: 1/3;
}

.button {
  grid-column: 2;
  margin-top: 10px;
}
</style>