<template>
    <v-col cols="3">
        <v-card>
            <v-card-title :title="pedido.cliente.nome">{{ pedido.cliente.nome }}</v-card-title>
            <span class="ms-5">Valor total: {{ pedido.valorTotal }}</span>
        </v-card>
        <Snackbar :snackbar="snackbar" />
    </v-col>
</template>
<script>
import Snackbar from '@/components/Snackbar.vue'
export default {
    name: "card-produto",
    components: {
        Snackbar
    },
    props: {
        pedido: {
            type: Object,
            default: () => ({
                cliente: {
                    nome: "",
                    telefone: "",
                    endereco: {
                        logradouro: "",
                        numero: "",
                        bairro: "",
                        complemento: "",
                        referencia: ""
                    }
                },
                listaProdutos: [],
                valorTotal: 0,
                formaPagamento: "pix"
            })
        }
    }
}
</script>