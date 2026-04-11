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

      const novaDespesa = {
        servico_id: servicoId,
        descricao: despesa.descricao,
        valor: despesa.valor
      }

      const token = localStorage.getItem("token");

      fetch("http://127.0.0.1:8000/api/despesas", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": `Bearer ${token}`
        },
        body: JSON.stringify(novaDespesa)
      })
        .then(res => {
          if (!res.ok) {
            throw new Error("Erro ao criar despesa");
          }
          return res.json();
        })
        .then(data => {
          console.log("Despesa salva:", data)

          this.$emit("adicionar-despesa", {
            servicoId,
            despesa: data
          })
        })
        .catch(err => {
          console.error(err)
          alert("Erro ao adicionar despesa")
        })
    },

    removerDespesa(servicoId, despesa) {

      console.log("remover despesa", servicoId, despesa)

      const token = localStorage.getItem("token");

      fetch(`http://127.0.0.1:8000/api/despesas/${despesa.id}`, {
        method: "DELETE",
        headers: {
          "Authorization": `Bearer ${token}`
        }
      })
        .then(res => {

          if (!res.ok) {
            throw new Error("Erro ao remover despesa");
          }

          return res.json();
        })
        .then(() => {

          // remove da tela
          this.$emit("remover-despesa", {
            servicoId,
            despesaId: despesa.id
          })

        })
        .catch(err => {
          console.error(err)
          alert("Erro ao remover despesa")
        })
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
          <span class="item">Horas: {{ servico.horas }}H </span>
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

          <button class="toggle-despesas" @click="toggleDespesas(servico.id)">
            Despesas
          </button>

          <button class="remover" @click="$emit('remover-servico', servico.id)">
            Remover
          </button>

        </div>

        <div v-if="servicoExpandido === servico.id">
          <DespesasServico :despesas="servico.despesas" @adicionar-despesa="nova => adicionarDespesa(servico.id, nova)"
            @remover-despesa="despesa => removerDespesa(servico.id, despesa)" />
        </div>
      </div>
    </div>
  </div>

</template>

<style></style>