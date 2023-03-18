<template>
    <div>
        <!-- // nome do produto -->
        <span>Produto: {{ produto.nome }}</span> |
        <!-- // quantidade pedida -->
        <span>Quantidade: {{ item.quantidade }}</span> |
        <!-- valor total -->
        <span>Total: {{ item.quantidade * produto.preco }}</span>
        <hr />
    </div>
</template>
<script>
export default {
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
                quantidade: "",
                preco: 0
            }
        }
    },
    mounted(){
        if(this.item.produtoId > 0){
            fetch('http://localhost:3000/produtos/' + this.item.produtoId, {
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
    
</style>