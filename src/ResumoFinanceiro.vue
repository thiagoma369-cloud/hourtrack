<script>
   export default {
      name: "ResumoFinanceiro",

      props: {
        mediaValorHora: Number,
        servicoMaisLucrativo: Object,
        totalBrutoGeral: Number,
        totalLiquidoGeral: Number,
        totalDespesasMes: Number,
        calcularTotal: Function
      }
   }
</script>

<template>

    <!-- RESUMO FINANCEIRO
     (Mostra métricas gerais) -->
    <div class="resumo">

        <h2>Resumo Geral:</h2>
        <hr>

        <!-- Média por hora -->
        <p>
            <strong>Média por Hora:</strong>
            R$ {{ mediaValorHora }}
        </p>
<hr>
         <!-- Serviço mais lucrativo -->
       <!--   <div v-if="servicoMaisLucrativo"> 
            <p><strong>Serviço Mais Lucrativo:</strong></p>
            <p>
                Contratante:
                {{ servicoMaisLucrativo.contratante }}
            </p>

            <p>
                Máquina:
                {{ servicoMaisLucrativo.maquina }}
            </p>
        
            <p>
                Total:
                R$ {{ calcularTotal(servicoMaisLucrativo) }}
            </p>
            
            <hr>
        </div>  -->

        
        <!-- Totais gerais -->
        <p>
            <strong>Total Bruto:</strong>
            R$ {{ totalBrutoGeral }}
        </p>
<hr>

        <p>
  <strong>Total de Despesas:</strong>
  R$ {{ totalDespesasMes }}
</p>

<hr>
        <p>
            <strong>Total Líquido:</strong>
            R$ {{ totalLiquidoGeral }}
        </p>
    </div>
</template>