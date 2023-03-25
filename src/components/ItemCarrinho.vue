<template>
    <tr>
        <td>{{produto.nome}}</td>
        <td>{{item.quantidade}}</td>
        <td>{{produto.preco.toLocaleString('pt-br',{style: 'currency', currency: 'BRL'})}}</td>
        <td>{{calcularValorTotal}}</td>
        <td><v-btn color="red" size="x-small" icon @click="removerProduto"><v-icon>mdi-delete</v-icon></v-btn></td>
        <snackbar :snackbar="snackbar" />
    </tr>
</template>
<script>
import Snackbar from '../components/Snackbar.vue'
export default {
    components: {
        Snackbar
    },
    props: {
        item: {
            type: Object,
            default: () => ({
                id: 0,
                quantidade: 0,
                produtoId: 0 
            })
        }
    },
    data(){
        return {
            produto: {
                id: 0,
                nome: "",
                quantidade: 0,
                preco: 0
            },
            snackbar: {
                ativo: false,
                mensagem: "Produto removido com sucesso!"
            }
        }
    },
    computed: {
        calcularValorTotal: function () {
            return (this.produto.preco * this.item.quantidade).toLocaleString('pt-br',{style: 'currency', currency: 'BRL'})
        }
    },
    methods: {
        removerProduto(){
            // adicionar novamente a quantidade
            fetch('http://localhost:50005/produtos/' + this.produto.id, {
                    method: "PUT",
                    headers: {"Content-type": "application/json;charset=UTF-8"},
                    body: JSON.stringify({
                        nome: this.produto.nome,
                        preco: this.produto.preco,
                        quantidade: Number.parseFloat(this.produto.quantidade) + Number.parseFloat(this.item.quantidade),
                        imagem: this.produto.imagem
                    })
            }).then(() => {
            }).catch(error => {
                this.snackbar.ativo = true
                this.snackbar.mensagem = error.message
            })

            fetch('http://localhost:50005/carrinhoDeCompras/' + this.item.id, {
                method: "DELETE",
                headers: {"Content-type": "application/json;charset=UTF-8"},
            })
            .then(() => {
                this.snackbar.ativo = true
                this.snackbar.mensagem = "Produto removido com sucesso!"
            }).catch(error => {
                console.log(error)
                this.snackbar.ativo = true
                this.snackbar.mensagem = "Não foi possível remover!"
            })

            this.$emit("atualizarLista", this.item.id)
        }
    },
    mounted(){
        if(this.item.produtoId > 0){
            fetch('http://localhost:50005/produtos/' + this.item.produtoId, {
                method: "GET",
                headers: {"Content-type": "application/json;charset=UTF-8"},
            })
            .then(response => response.json())
            .then(dados => {
                this.produto = dados
            }).catch(error => {
                console.log(error)
            })
        }
    }
}
</script>
<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 70%;

}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>