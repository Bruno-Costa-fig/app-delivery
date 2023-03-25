<template>
  <div>
    <NavbarVue />
    <v-btn class="mb-5 ms-5" color="green" to="/novo-produto"> + novo</v-btn>
    <v-container>
      <v-flex class="d-flex flex-wrap">
        <CardProduto class="col-4" v-for="produto in produtos" :produto="produto" :key="produto.id" />
      </v-flex>
    </v-container>
  </div>
</template>
<script>
import NavbarVue from '@/components/Navbar.vue'
import CardProduto from '../../components/CardProduto.vue'
export default {
  components: {
    NavbarVue,
    CardProduto
},
  data() {
    return {
      produtos: []
    }
  },
  mounted() {
    fetch('http://localhost:50005/produtos', {
      method: "GET",
      headers: {"Content-type": "application/json;charset=UTF-8"},
    })
    .then(response => response.json())
    .then(response => {
      this.produtos = response
    }).catch(error => {
      console.log(error)
    })
  }
}
</script>
<style>
  
</style>