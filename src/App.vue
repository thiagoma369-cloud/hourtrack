<script>
import FormularioServico from './FormularioServico.vue';
import ResumoFinanceiro from './ResumoFinanceiro.vue';
import ListaServicos from './ListaServicos.vue';
import FiltrosServicos from './FiltrosServicos.vue';

export default {
  name: "App",
  components: {
    FormularioServico,
    ResumoFinanceiro,
    ListaServicos,
    FiltrosServicos
  },

  /* DADOS DA APLICAÇÃO */
  data() {
    return {

      // Tipos fixos disponíveis no formulário
      tiposServico: [
        { nome: "Arado" },
        { nome: "Roço" },
        { nome: "Plantio" },
        { nome: "Derrubada" }
      ],

      // Estados de filtro
      filtroTipo: "",
      filtroContratante: "",
      filtroMaquina: "",
      filtroData: "",

      // Lista principal de serviços registrados
      servicos: []
    }
  },


  /* COMPUTED (DERIVAÇÕES)  */
  computed: {

    // Soma total bruto geral
    totalBrutoGeral() {
      return this.servicos.reduce((total, servico) => {
        return total + (servico.valorHora * servico.horas)
      }, 0)
    },

    // total de despesas por mês
    totalDespesasMes() {
      return this.servicos.reduce((total, servico) => {
        if (!servico.despesas) return total
        const totalDespesasServico = servico.despesas.reduce((soma, despesa) => soma + Number(despesa.valor), 0)
        return total + totalDespesasServico
      }, 0)
    },

    // Soma total líquido geral
    totalLiquidoGeral() {
      return this.servicos.reduce((total, servico) => {
        return total + this.calcularTotal(servico)
      }, 0)
    },

    // Retorna apenas tipos que já foram usados, para nao repetir
    tiposUnicos() {
      const tipos = this.servicos.map(servico => servico.tipo)
      return [...new Set(tipos)]
    },

    // Retorna maquinas no select do filtro sempre que uma nova é adc
    maquinasUnicas() {
      return [...new Set(this.servicos.map(s => s.maquina))]
    },

    // Lista filtrada + ordenada
    servicosFiltrados() {
      let lista = [...this.servicos] // cópia para evitar mutação

      // Filtro por tipo
      if (this.filtroTipo) {
        lista = lista.filter(servico =>
          servico.tipo === this.filtroTipo
        )
      }

      // Filtro por contratante
      if (this.filtroContratante) {
        lista = lista.filter(servico =>
          servico.contratante
            .toLowerCase()
            .includes(this.filtroContratante.toLowerCase())
        )
      }

      // Filtro por Máquina
      if (this.filtroMaquina) {
        lista = lista.filter(servico =>
          servico.maquina === this.filtroMaquina
        )
      }

      // Filtro por data 
      if (this.filtroData) {
        lista = lista.filter(servico =>
          servico.data === this.filtroData
        )
      }
      // Ordenação por data (mais recente primeiro)
      return lista.sort((a, b) =>
        new Date(b.data) - new Date(a.data)
      )

    },

    // Média geral de valor por hora
    mediaValorHora() {
      if (!this.servicos.length) return 0

      const totalHoras = this.servicos.reduce((total, servico) => {
        return total + servico.horas
      }, 0)

      const totalBruto = this.totalBrutoGeral

      return totalHoras
        ? Number((totalBruto / totalHoras).toFixed(2))
        : 0
    }

    // Retorna o serviço com maior valor líquido
    /* servicoMaisLucrativo() {
      if (!this.servicos.length) return null
      
      return this.servicos.reduce((maior, atual) => {
        const totalAtual = this.calcularTotal(atual)
        const totalMaior = this.calcularTotal(maior)

        return totalAtual > totalMaior ? atual : maior
      })
    } */
  },

  /* MÉTODOS (AÇÕES) */
  methods: {
     //salvar serviço no banco
    adicionarServico(novoServico) {
      fetch("http://127.0.0.1:8000/api/servicos", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(novoServico)
      })
        .then(res => res.json())
        .then(data => {
          this.servicos.push(data)
        })
    },

    // Remove serviço pelo id
    removerServico(id) {
      this.servicos = this.servicos.filter(
        servico => servico.id !== id
      )
    },

    // Calcula total líquido de um serviço específico
    calcularTotal(servico) {
      const bruto = servico.valorHora * servico.horas
      const despesas = servico.despesas || [] //se não existir despezas, use um array vazio
      const totalDespesas = despesas.reduce((total, despesa) => total + Number(despesa.valor), 0)
      return bruto - totalDespesas
    },

    adicionarDespesa({ servicoId, despesa }) {
      const servico = this.servicos.find(s => s.id === servicoId)

      if (!servico.despesas) {
        servico.despesas = []
      }

      servico.despesas.push(despesa)
    },

    removerDespesa({ servicoId, index }) {
      const servico = this.servicos.find(s => s.id === servicoId)

      if (servico && servico.despesas) {
        servico.despesas = servico.despesas.filter((_, i) => i !== index)
      }
    }
  },

  /* CICLO DE VIDA */
  mounted() {
    fetch("http://127.0.0.1:8000/api/servicos")
      .then(res => res.json())
      .then(data => {
        this.servicos = data.map(servico => ({
          ...servico,
          valorHora: servico.valor_hora
        }))
      })
  },

  /* OBSERVADORES */
  watch: {
    servicos: {
      handler(novoValor) {
        localStorage.setItem("servicos", JSON.stringify(novoValor))
      },
      deep: true
    }
  }
}
</script>


<template>

  <!-- CONTAINER PRINCIPAL 
  (Estrutura geral da aplicação) -->
  <div class="container">

    <!-- TÍTULO DA APLICAÇÃO -->
    <h1>Relatório de Serviços</h1>


    <FormularioServico :tiposServico="tiposServico" @adicionar-servico="adicionarServico" />

    <hr /> <br>


    <ResumoFinanceiro :mediaValorHora="mediaValorHora" 
      :totalBrutoGeral="totalBrutoGeral" 
      :totalLiquidoGeral="totalLiquidoGeral" 
      :totalDespesasMes="totalDespesasMes"
      :calcularTotal="calcularTotal" />


    <FiltrosServicos :filtroTipo="filtroTipo" :filtroContratante="filtroContratante" :filtroMaquina="filtroMaquina"
      :filtroData="filtroData" :tiposUnicos="tiposUnicos" :maquinasUnicas="maquinasUnicas"
      @atualizar-tipo="filtroTipo = $event" @atualizar-contratante="filtroContratante = $event"
      @atualizar-maquina="filtroMaquina = $event" @atualizar-data="filtroData = $event" />

    <ListaServicos :servicos="servicosFiltrados" :calcularTotal="calcularTotal" @remover-servico="removerServico"
      @adicionar-despesa="adicionarDespesa" @remover-despesa="removerDespesa" />

  </div>
</template>

<style>
/* base visual da página */
body {
  background: linear-gradient(135deg, #e8f5e9, #ffffff);
  color: #1c1c1c;
}

/* layout dos filtros */
.filtros {
  display: flex;
  gap: 15px;
  margin-bottom: 20px;
}

/* container central da aplicação */
.container {
  max-width: 900px;
  margin: 40px auto;
  padding: 20px;
}

/* espaçamento entre as principais seções */
.formulario,
.resumo,
.lista {
  margin-bottom: 40px;
}

.lista {
  max-width: 900px;
  margin: 30px auto;

  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

/* formulário organizado em duas colunas */
.formulario {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
}

/* elementos que ocupam largura total no grid */
.formulario input[type="date"],
.formulario button,
.resultado {
  grid-column: span 2;
}

/* padrão visual para campos e botões */
input,
select,
button {
  padding: 10px;
  font-size: 14px;
}

/* estilo visual dos campos */
input,
select {
  background: linear-gradient(135deg, #e8f5e9, #ffffff);
  color: #1c1c1c;
}


/* botão principal */
/* estilo base de todos botões */

button {
  padding: 10px 14px;
  font-size: 14px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.2s;
}

/* botao principal */
.adicionar {
  background: #2e7d32;
  color: white;
}

/* efeito hover do botão */
.adicionar:hover {
  background: #1b5e20;
}

/* restante dos botoes */

.toggle-despesas {
  background: #e5e7eb;
  border: none;
  padding: 6px 10px;
  border-radius: 6px;
  cursor: pointer;
  color: #333
}

.toggle-despesas:hover {
  background: #d5d5d5;
}

.remover {
  background: #ef4444;
  color: white;
  border: none;
  padding: 6px 10px;
  border-radius: 6px;
  cursor: pointer;
}

.remover-despesa {
  background: #ef4444;
  color: white;
  border: none;
  padding: 2px 2px;
  border-radius: 6px;
  cursor: pointer;
}

.remover:hover {
  background: #b71c1c;
}


/* títulos */
h1 {
  font-size: 28px;
  margin-bottom: 30px;
  color: #1b5e20;
}

h2 {
  font-size: 20px;
  margin-bottom: 15px;
}

/* bloco de resumo com destaque lateral */
.resumo {
  padding: 25px;
  border: 1px solid #ddd;
  border-left: 6px solid #2e7d32;
  border-radius: 8px;
  background-color: #f1f8f4;
}

.resumo strong {
  font-size: 16px;
  color: #1b5e20;
}

/* card de serviço (estrutura + visual juntos) */
.card {
  padding: 16px;
  border: 1px solid #2e7d32;
  border-radius: 8px;
  background-color: white;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.04);
  margin-bottom: 12px;
  background: white;
  border-radius: 10px;
  padding: 16px;

  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);

  display: flex;
  flex-direction: column;
  gap: 8px;
}

.despesas {
  margin-top: 10px;
  padding: 10px;
  background: #fafafa;
  border-radius: 6px;
}

@media (max-width:700px) {

  .formulario {
    grid-template-columns: 1fr;
  }

  .formulario input[type="date"],
  .formulario button,
  .resultado {
    grid-column: span 1;
  }

  .filtros {
    flex-direction: column;
  }

  .lista {
    grid-template-columns: 1fr;
  }

}

.linha-servico-topo {

  font-size: 18px;
  color: #1f2937;

}

.linha-servico-info {

  font-size: 16px;
  color: #555;
  margin-bottom: 4px;
  display: flex;
  justify-content: space-around;
}


.linha-servico-financeiro {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.total {
  font-weight: bold;
  color: #16a34a;
  font-size: 16px;
}

.data {
  font-size: 13px;
  color: #6b7280;
}

.linha-servico-acoes {

  display: flex;
  gap: 10px;
  margin-top: 6px;
  justify-content: center;
}
</style>