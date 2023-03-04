<template>
  <div>
    <NavbarVue />
    {{ produtos }}
    <v-btn class="mb-5 ms-5" color="green" to="/novo-produto"> + novo</v-btn>
  </div>
</template>
<script>
import NavbarVue from '@/components/Navbar.vue'
export default {
  components: {
    NavbarVue
  },
  data() {
    return {
      produtos: []
    }
  },
  mounted() {
    fetch('http://localhost:3000/produtos', {
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