<script>
  export default {
    name: "FiltrosServicos",

    props: {
        filtroTipo: String,
        filtroContratante: String,
        filtroMaquina: String,
        filtroData: String,
       
        tiposUnicos: Array,
        maquinasUnicas: Array
    },

    methods: {
        atualizarTipo(valor) {
            this.$emit("atualizar-tipo", valor)
        },

        atualizarContratante(valor) {
            this.$emit("atualizar-contratante", valor)
        },

        atualizarMaquina(valor) {
    this.$emit("atualizar-maquina", valor)
        },

        atualizarData(valor) {
            this.$emit("atualizar-data", valor)
        }
      }
    }
</script>

<template>
     <!-- FILTROS
  (Controlam a visualização da lista) -->
    <div class="filtros">

      <h2>Filtros</h2>

       <!-- Filtro por contratante -->
      <input type="text" 
      placeholder="Buscar por contratante..."
       :value="filtroContratante" 
       @input="atualizarContratante($event.target.value)"
       />

        <!-- Filtro por tipo -->
      <select 
      :value="filtroTipo"
      @change="atualizarTipo($event.target.value)">
        
        <option value="">
          Todos os tipos
        </option>

        <!-- Tipos gerados dinamicamente
             com base nos serviços já cadastrados -->
        <option 
        v-for="tipo in tiposUnicos" 
        :key="tipo" 
        :value="tipo"
        >
          {{ tipo }}
        </option>
      </select>
      
     <!-- filtro por maquina -->
      <select
  :value="filtroMaquina"
  @change="atualizarMaquina($event.target.value)"
>

  <option value="">
    Todas as máquinas
  </option>

  <option
    v-for="maq in maquinasUnicas"
    :key="maq"
    :value="maq"
  >
    {{ maq }}
  </option>

</select>

   <!-- filtro por data -->

   <input 
type="date"
  :value="filtroData"
  @input="atualizarData($event.target.value)"
  placeholder="Filtrar por Data"
  >
      
    </div>
</template>