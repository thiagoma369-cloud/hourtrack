<script>
export default {
    name: "DespesasServico",

    props: {
        despesas: Array
    },

    data() {
        return {
            descricao: "",
            valor: null
        }
    },

    methods: {
        adicionarDespesa() {
            if (!this.descricao || this.valor === null) return

            const novaDespesa = {
                descricao: this.descricao,
                valor: this.valor
            }

            this.$emit("adicionar-despesa", novaDespesa)

            this.descricao = ""
            this.valor = null
        },

        removerDespesa(despesa) {
            console.log("removida com sucesso", despesa)
            this.$emit("remover-despesa", despesa)
        }
    }
}
</script>

<template>
    <div class="despesas">
        <h4>Despesas</h4>

        <div v-for="(despesa, index) in despesas" :key="index" class="despesa-item">
            <p>
                {{ despesa.descricao }} - R$ {{ despesa.valor }}
            </p>

            <button class="remover-despesa"
             @click="removerDespesa(despesa)">
                Remover
            </button>
        </div>

        <hr>

        <input type="text" placeholder="Descrição da despesa" v-model="descricao" />

        <input type="number" placeholder="Valor" v-model.number="valor" />

        <button class="adicionar" @click="adicionarDespesa">
            Adicionar Despesa
        </button>
    </div>
</template>