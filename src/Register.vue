<script>
import API_URL from './services/api';

export default {
  name: "Register",

  data() {
    return {
      name: "",
      email: "",
      password: ""
    }
  },

  methods: {
    fazerCadastro() {

      fetch(`${API_URL}/register`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          name: this.name,
          email: this.email,
          password: this.password
        })
      })
        .then(res => res.json())
        .then(data => {

          console.log("Usuário criado:", data)

          alert("Cadastro realizado com sucesso!")

          // avisa o App.vue
          this.$emit("cadastro-sucesso")

        })
        .catch(err => {
          console.error(err)
          alert("Erro ao cadastrar")
        })
    }
  }
}
</script>

<template>
  <div class="container">

    <h1>Cadastro</h1>

    <div class="formulario">

      <input
        type="text"
        placeholder="Nome"
        v-model="name"
      />

      <input
        type="email"
        placeholder="Email"
        v-model="email"
      />

      <input
        type="password"
        placeholder="Senha"
        v-model="password"
      />

      <button class="adicionar" @click="fazerCadastro">
        Cadastrar
      </button>

    </div>

    <br>

    <button class="toggle-despesas" @click="$emit('voltar-login')">
      Já tenho conta
    </button>

  </div>
</template>