<script>
import API_URL from './services/api';

export default {
  name: "Login",

  data() {
    return {
      email: "",
      password: ""
    }
  },

  methods: {
    fazerLogin() {
      fetch(`${API_URL}/login`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          email: this.email,
          password: this.password
        })
      })
        .then(res => res.json())
        .then(data => {

          console.log("Resposta do backend:", data)

          // verifica se tem token (login valido)
          if (!data.token) {
            alert("Email ou senha inválidos")
            return
          }

          //  salva token
          localStorage.setItem("token", data.token)

          // avisa o App.vue que logou
          this.$emit("login-sucesso", data)

          localStorage.setItem("usuario", JSON.stringify(data.user))

        })
    }
  }
}
</script>

<template>
  <div class="container">

    <h1>HourTrack</h1>

    <div class="formulario">

      <input type="email" placeholder="Email" v-model="email" />

      <input type="password" placeholder="Senha" v-model="password" />

      <button class="adicionar" @click="fazerLogin">
        Entrar
      </button>

      <button class="toggle-despesas" @click="$emit('ir-cadastro')">
        Criar conta
      </button>

    </div>
  </div>
</template>