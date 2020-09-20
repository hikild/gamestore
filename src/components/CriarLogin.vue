<template>
  <section>
    <h2>Crie sua conta</h2>
    <NotificacaoError :erros="erros" />
    <transition mode="out-in">
      <button v-if="!criar" class="btn" @click="criar = true">Criar conta</button>
      <UsuarioFormulario v-else>
        <button class="btn btn-form" @click.prevent="criarUsuario">Criar usuário</button>
      </UsuarioFormulario>
    </transition>
  </section>
</template>

<script>
import UsuarioFormulario from "@/components/UsuarioFormulario.vue";
export default {
  name: "CriarLogin",
  data() {
    return {
      erros: [],
      criar: false,
    };
  },
  components: {
    UsuarioFormulario,
  },
  methods: {
    async criarUsuario() {
      try {
        await this.$store.dispatch("criarUsuario", this.$store.state.usuario);
        await this.$store.dispatch("logarUsuario", this.$store.state.usuario);
        await this.$store.dispatch("getUsuario");
        this.$router.push({ name: "usuario" });
      } catch (error) {
        this.erros.push(error.response.data.message);
      }
    },
  },
};
</script>

<style scoped>
h2 {
  text-align: center;
  margin-top: 40px;
  margin-bottom: 10px;
}

.btn {
  width: 100%;
  max-width: 300px;
  margin-left: auto;
  margin-right: auto;
}

.btn-form {
  max-width: 100%;
}
</style>