<script>
import DespesasServico from './Despesas.vue';

export default {
  name: "ListaServicos",

  components: {
    DespesasServico
  },

  props: {
    servicos: Array,
    calcularTotal: Function
  },

  data() {
    return {
      servicoExpandido: null
    }
  },

  methods: {
    emitirRemocao(id) {
      this.$emit("remover-servico", id)
    },

    toggleDespesas(id) {
       if (this.servicoExpandido === id) {
      this.servicoExpandido = null
    } else {
      this.servicoExpandido = id
    }
    },

   adicionarDespesa(servicoId, despesa) {
  console.log("lista recebeu despesa", servicoId, despesa)

  this.$emit("adicionar-despesa", { servicoId, despesa })
},

    removerDespesa(servicoId, index) {
       this.$emit("remover-despesa", { servicoId, index })
    }
  }
}
</script>

<template>
  <!-- LISTA DE SERVIÇOS
  (Renderiza serviços filtrados) -->
 
  
  <div>
    
    <h2>Serviços Registrados</h2>

    <div class="lista">

    <!-- Usa o computed servicosFiltrados -->
    <div v-for="servico in servicos" :key="servico.id" class="card">

      
       <!-- linha 1 -->
  <div class="linha-servico-topo">
    <strong>
     {{ servico.contratante }} — {{ servico.maquina }} 
    </strong>
  </div>

  <!-- linha 2 -->
  <div class="linha-servico-info">
   <span class="item">Tipo: {{ servico.tipo }} </span>
    <span class="item">Horas: {{  servico.horas}}H </span>
   <span class="item"> R$ {{ servico.valorHora }}/H</span>
  </div>

  <!-- linha 3 -->
  <div class="linha-servico-financeiro">
    <span class="data">
     {{ servico.data }} 
    </span>

    <span class="total">
   R$ {{ calcularTotal(servico) }}
    </span>
  </div>


      <!-- Botão remover -->
      <div class="linha-servico-acoes">

      <button
      class="toggle-despesas"
      @click="toggleDespesas(servico.id)"
    >
      Despesas
    </button>

    <button
      class="remover"
      @click="$emit('remover-servico', servico.id)"
    >
      Remover
    </button>

    </div>

      <div v-if="servicoExpandido === servico.id">
        <DespesasServico :despesas="servico.despesas" @adicionar-despesa="nova => adicionarDespesa(servico.id, nova)"
          @remover-despesa="index => removerDespesa(servico.id, index)" 
          />
      </div>
    </div>
  </div>
  </div>

</template>

<style>

</style>