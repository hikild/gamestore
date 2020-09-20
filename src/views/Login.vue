<template>
  <section class="login">
    <h1>Login</h1>
    <form>
      <label for="email">Email</label>
      <input type="email" name="email" id="email" v-model="login.email" />
      <label for="senha">Senha</label>
      <input type="password" name="senha" id="senha" v-model="login.senha" />
      <button class="btn" @click.prevent="logar">Logar</button>
      <NotificacaoError :erros="erros" />
    </form>
    <p class="esqueceu-senha">
      <a
        href="http://gamestoreapi.local/wp-login.php?action=lostpassword"
        target="_blank"
      >Esqueceu sua senha? Clique aqui.</a>
    </p>
    <CriarLogin />
  </section>
</template>

<script>
import CriarLogin from "@/components/CriarLogin.vue";

export default {
  name: "Login",
  data() {
    return {
      login: {
        email: "",
        senha: "",
      },
      erros: [],
    };
  },
  components: {
    CriarLogin,
  },
  methods: {
    logar() {
      this.erros = [];
      this.$store
        .dispatch("logarUsuario", this.login)
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
.login {
  max-width: 500px;
  margin: 0 auto;
  padding: 0 20px;
}

h1 {
  text-align: center;
  font-size: 2rem;
  margin-top: 50px;
  margin-bottom: 20px;
  color: #a635ff;
}
form {
  display: grid;
}

.btn {
  width: 100%;
  max-width: 300px;
  margin-left: auto;
  margin-right: auto;
}

.esqueceu-senha {
  text-align: center;
  margin: 20px auto 0 auto;
}

.esqueceu-senha a:hover {
  color: #dbf01a;
  text-decoration: underline;
}
.esqueceu-senha a {
  color: #e83766;
}
</style>