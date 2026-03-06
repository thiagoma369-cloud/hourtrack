<script>
export default {
  name: "FormularioServico",

  props: {
    tiposServico: Array
  },

  data() {
    return {

      // Campos do formulário
      data: "",
      maquina: "",
      contratante: "",
      tipoSelecionado: "",
      valorHora: null,
      horas: null
    }
  },

  computed: {
    // Calcula o total do serviço enquanto está sendo digitado
    totalServico() {
      return (this.valorHora || 0) * (this.horas || 0)
    } 
  },

  methods: {
    // Adiciona novo serviço à lista
    adicionarServico() {
      if (
        !this.data ||
        !this.maquina ||
        !this.contratante ||
        !this.tipoSelecionado ||
        this.valorHora === null ||
        this.horas === null
      ) {
        alert("Preencha todos os campos obrigatórios.")
        return
      }

      const novoServico = {
        id: Date.now(),
        data: this.data,
        maquina: this.maquina,
        contratante: this.contratante,
        tipo: this.tipoSelecionado,
        valorHora: this.valorHora,
        horas: this.horas,
        despesas: []
      }

      this.$emit("adicionar-servico", novoServico)
      this.limparFormulario()
    },

    // Limpa campos do formulário
    limparFormulario() {
      this.data = ""
      this.maquina = ""
      this.contratante = ""
      this.tipoSelecionado = ""
      this.valorHora = null
      this.horas = null
    }
  }
}
</script>


<template>
  <div class="formulario">

    <!-- Data do serviço -->
    <input type="date" v-model="data">

    <!-- Nome da máquina utilizada -->
    <input type="text" placeholder="Nome da Máquina" v-model.trim="maquina">

    <!-- Nome do contratante -->
    <input type="text" placeholder="Nome do Contratante" v-model.trim="contratante">

    <!-- Seleção do tipo de serviço -->
    <select v-model="tipoSelecionado">
      <option disabled value="">
        Selecione o serviço
      </option>

      <!-- Tipos fixos definidos no data() -->
      <option v-for="tipo in tiposServico" :key="tipo.nome" :value="tipo.nome">
        {{ tipo.nome }}
      </option>
    </select>

    <!-- Valor por hora -->
    <input type="number" placeholder="Valor por Hora" v-model.number="valorHora" />

    <!-- Quantidade de horas -->
    <input type="number" placeholder="Horas Trabalhadas" v-model.number="horas" />

    <!-- Resultado parcial enquanto digita -->
    <div class="resultado" v-if="valorHora && horas">
      <strong>
        Total Serviço: R$ {{ totalServico }}
      </strong>
    </div>

    <!-- Botão para adicionar -->
    <button class="adicionar" @click="adicionarServico">
      Adicionar Serviço
    </button>
  </div>
</template>