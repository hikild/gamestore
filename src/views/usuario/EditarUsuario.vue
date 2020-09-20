<template>
  <section>
    <NotificacaoError :erros="erros" />
    <UsuarioFormulario>
      <button class="btn" @click.prevent="atualizarUsuario">Atualizar informações</button>
    </UsuarioFormulario>
  </section>
</template>

<script>
import UsuarioFormulario from "@/components/UsuarioFormulario.vue";
import { api } from "@/services.js";
export default {
  name: "EditarUsuario",
  components: {
    UsuarioFormulario,
  },
  data() {
    return {
      erros: [],
    };
  },
  methods: {
    atualizarUsuario() {
      this.erros = [];
      api
        .put("/usuario", this.$store.state.usuario)
        .then(() => {
          this.$store.dispatch("getUsuario");
          this.$router.push({ name: "usuario" });
        })
        .catch((error) => {
          this.erros.push(error.response.data.message);
        });
    },
  },
};
</script>

<style scoped>
</style>