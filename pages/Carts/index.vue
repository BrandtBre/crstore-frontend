<template>
  <v-container>
    <v-container>
      <h1>Carrinho</h1>
      <hr>
    </v-container>
    <v-container>
      <v-row dense> 
        <v-col cols="12">
          <v-card
            v-for="(cart, i) in carts"
              :key="i"
              router
              exact
              style="margin: 10px; border: 5px solid #e2e3ca "
          >
            <v-card-title class="text-h5">
              Meu Carrinho
            </v-card-title>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-container>
</template>

<script>
export default {
  layout: "userLayout",
  name: 'ConsultaAutoresPage',
  data () {
    return {
      headers: [
        {
          text: 'Codigo', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'id', //é o dado que essa coluna vai receber
        },
        {
          text: 'Produtos',
          align: 'center',
          sortable: false,
          value: 'items',
        },
        
        {text: "", value: "actions"}
      ],
      carts: [],
      item: {
        id: null,
        name: null,
        price: null,
        categoryId: null,
      },
    }
  },
  created () {
    this.getCartsByUserId()
  },
  
  methods: {
    async getCartsByUserId () {
      let response = await this.$api.get('/carts/');
      this.carts = response.data
      
    },

    async destroy (category) {
      try {
        if (confirm(`Do you wish to delete the category: id ${category.id} - ${category.type}?`)) {
          let response = await this.$api.post('/categories/destroy', { id: category.id });
          this.$toast(response.message)
          this.getCategories();
        }
      } catch (error) {
        this.$toast.error('An error has ocurred while trying to delete the category =(');
      }
    },
    async editItem (category) {
      this.$router.push({
        name: 'Categories-index',
        params: { id: category.id }
      });
    }
  }
}
</script>

