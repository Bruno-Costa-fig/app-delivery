<template>
  <div>
    <NavbarVue />
    <h1>Delivery App</h1>
    <CardPedido v-for="pedido in listaPedidos" :key="pedido.id" :pedido="pedido" />
  </div>
</template>
<script>
import CardPedido from '../components/CardPedido.vue'
import NavbarVue from '../components/Navbar.vue'

export default {
  components: {
    NavbarVue,
    CardPedido
},
  data(){
    return {
      listaPedidos: []
    }
  },
  mounted(){
        fetch('http://localhost:50005/pedidos', {
            method: "GET",
            headers: {"Content-type": "application/json;charset=UTF-8"},
        })
        .then(response => response.json())
        .then(dados => {
            this.listaPedidos = dados
        }).catch(error => {
            console.log(error)
        })
    }
}
</script>
<style>
  
</style>
