<template>
    <v-col cols="3">
        <v-card>
            <v-card-title :title="produto.nome">{{produto.nome}}</v-card-title>
            <span class="ms-5">Quant. Disponível {{ produto.quantidade }}</span>
            <img width="200" :src="produto.imagem" :alt="produto.nome">
            <div class="pa-3">
                <v-col cols="12">
                    <v-text-field type="number" v-model="quantidade" label="Quantidade" />
                </v-col>
                <span class="ms-5 me-5">R$ {{ Number.parseFloat(produto.preco).toFixed(2) }}</span> 
                <v-btn @click="adicionarProdutoCarrinho" title="adicionar ao carrinho" icon class="ms-5"><v-icon>mdi-basket</v-icon></v-btn>
            </div>

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
        produto: {
            type: Object,
            default: () => ({
                id: 0,
                nome: "",
                preco: 0,
                quantidade: 0,
                imagem: ""
            })
        }
    },
    data(){
        return {
            quantidade: 1,
            snackbar: {
                ativo: false,
                mensagem: "Produto adicionado ao carrinho com sucesso!"
            }
        }
    },  
    methods: {
        adicionarProdutoCarrinho(){
            if(this.quantidade > 0){
                /* 
                    vamos adicionar apenas o id do produto e a quantidade pedida
                */
                fetch('http://localhost:50005/carrinhoDeCompras', {
                    method: "POST",
                    headers: {"Content-type": "application/json;charset=UTF-8"},
                    body: JSON.stringify({
                        produtoId: this.produto.id,
                        quantidade: this.quantidade
                    })
                }).then(() => {
                    this.snackbar.ativo = true,
                    this.snackbar.mensagem = "Produto adicionado ao carrinho com sucesso!"
                }).catch(error => {
                    this.snackbar.ativo = true
                    this.snackbar.mensagem = error.message
                })

                fetch('http://localhost:50005/produtos/' + this.produto.id, {
                    method: "PUT",
                    headers: {"Content-type": "application/json;charset=UTF-8"},
                    body: JSON.stringify({
                        id: this.produto.id,
                        nome: this.produto.nome,
                        preco: this.produto.preco,
                        quantidade: this.produto.quantidade - this.quantidade,
                        imagem: this.produto.imagem

                    })
                }).then(() => {
                    this.produto.quantidade = this.produto.quantidade - this.quantidade
                }).catch(error => {
                    this.snackbar.ativo = true
                    this.snackbar.mensagem = error.message
                })
            }
        }
    }
}
</script>
<style>
    
</style>