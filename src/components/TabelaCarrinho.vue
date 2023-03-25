<template>
    <div class="container-tabela">
        <table v-if="listaCarrinhoDeCompras.length > 0">
            <tr>
                <th>Produto</th>
                <th>Quantidade</th>
                <th>Val. unit.</th>
                <th>Val. total</th>
                <th>Ações</th>
            </tr>
            <ItemCarrinho 
                v-for="produto in listaCarrinhoDeCompras" 
                :item="produto" 
                :key="produto.id"
                @atualizarLista="atualizarLista"
            />
        </table>
    </div>
</template>
<script>
import ItemCarrinho from './ItemCarrinho.vue'
export default {
    components: {
        ItemCarrinho
    },
    data(){
        return {
            listaCarrinhoDeCompras: []
        }
    },
    methods: {
        atualizarLista(id){
            this.listaCarrinhoDeCompras = this.listaCarrinhoDeCompras.filter(item => item.id != id)
        }
    },
    mounted(){
        fetch('http://localhost:50005/carrinhoDeCompras', {
            method: "GET",
            headers: {"Content-type": "application/json;charset=UTF-8"},
        })
        .then(response => response.json())
        .then(dados => {
            this.listaCarrinhoDeCompras = dados
        }).catch(error => {
            console.log(error)
        })
    }
    
}
</script>
<style>
    .container-tabela {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        margin-bottom: 20px;
    }
</style>