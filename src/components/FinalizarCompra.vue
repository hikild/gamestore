<template>
  <section>
    <h2>Endereço de envio</h2>
    <NotificacaoError :erros="erros" />
    <UsuarioFormulario>
      <button class="btn" @click.prevent="finalizarCompra">Finalizar compra</button>
    </UsuarioFormulario>
  </section>
</template>

<script>
import UsuarioFormulario from "@/components/UsuarioFormulario.vue";
import { api } from "@/services.js";
import { mapState } from "vuex";
export default {
  name: "FinalizarCompra",
  components: {
    UsuarioFormulario,
  },
  data() {
    return {
      erros: [],
    };
  },
  props: ["produto"],
  computed: {
    ...mapState(["usuario"]),
    compras() {
      return {
        comprador_id: this.usuario.email,
        vendedor_id: this.produto.usuario_id,
        produto: this.produto,
        endereco: {
          cep: this.usuario.cep,
          rua: this.usuario.rua,
          numero: this.usuario.numero,
          bairro: this.usuario.bairro,
          cidade: this.usuario.cidade,
          estado: this.usuario.estado,
        },
      };
    },
  },
  methods: {
    fazerTransacao() {
      return api.post("/transacao", this.compras).then(() => {
        this.$router.push({ name: "compras" });
      });
    },
    async criarUsuario() {
      try {
        await this.$store.dispatch("criarUsuario", this.$store.state.usuario);
        await this.$store.dispatch("logarUsuario", this.$store.state.usuario);
        await this.$store.dispatch("getUsuario");
        await this.fazerTransacao();
      } catch (error) {
        this.erros.push(error.response.data.message);
      }
    },
    finalizarCompra() {
      this.erros = [];
      if (this.$store.state.login) {
        this.fazerTransacao();
      } else {
        this.criarUsuario();
      }
    },
  },
};
</script>

<style scoped>
h2 {
  margin-top: 40px;
  margin-bottom: 20px;
}

.btn {
  background: #e83766;
}
</style>