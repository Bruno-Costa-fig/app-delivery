<template>
  <div>
    <NavbarVue />
    <h1 class="ms-5">Criar novo produto</h1>
    <v-container>
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field v-model="produto.nome" label="Nome do produto" required />
        </v-col>
        <v-col cols="12" md="6">
          <v-text-field v-model="produto.valor" type="number" label="Valor do produto" required />
        </v-col>
      </v-row>
    </v-container>
    <v-btn class="mb-5 ms-5" color="green" @click="cadastrarProduto">Cadastrar</v-btn>
    <snackbar :snackbar="snackbar" />
  </div>
</template>
<script>
import NavbarVue from '../components/Navbar.vue'
import Snackbar from '../components/Snackbar.vue'
export default {
  components: {
    NavbarVue,
    Snackbar
  },
  data(){
    return {
      produto: {
        nome: "",
        valor: 0
      },
      snackbar: {
        ativo: false,
        mensagem: "Pedido cadastrado com sucesso!"
      }
    }
  },
  methods: {
    cadastrarProduto(){
      if(!this.produto.nome || !this.produto.valor){
        this.snackbar.ativo = true
        this.snackbar.mensagem = 'Por favor, preencha todos os campos!'
      }else{
        // lógica para cadastrar o produto
        this.snackbar.ativo = true
        this.snackbar.mensagem = 'Produto cadastrado com sucesso!'
      }
    }
  }
}
</script>
<style>
  
</style>