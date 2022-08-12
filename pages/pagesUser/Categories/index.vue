<template>
  <v-container>
    <v-container>
      <v-container style="text-align: center">
        <h1>Categorias</h1>
        <hr>
      </v-container>
      <v-container>
        <v-row>
          <v-card
            v-for="(category, i) in categories"
              :key="i"
              router
              exact
              style="margin: 10px; border: 5px solid #e2e3ca "
          >
            <v-img
              max-width="250px"
              max-height="250px"
              src="https://cdn.pixabay.com/photo/2020/06/26/16/04/chef-5343214_960_720.png"   
            ></v-img>
            <v-row>
              <v-card-title>
                {{ category.type }}
              </v-card-title>
              <v-card-actions>
                <v-btn
                  color="green lighten-2"
                  text
                  @click="getItemsByid(category.id)"
                >Procurar</v-btn>
              </v-card-actions>
            </v-row>
          </v-card>
        </v-row>
      </v-container>
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
          text: 'Categoria',
          align: 'center',
          sortable: false,
          value: 'type',
        },
        
        {text: "", value: "actions"}
      ],
      categories: [],
      item: {
        id: null,
        name: null,
        price: null,
        categoryId: null,
      },
    }
  },
  created () {
    this.getCategories()
  },
  
  methods: {
    async getCategories () {
      this.categories = await this.$axios.$get('http://localhost:3333/categories');
    },

    async getItemsByid (id) {
      this.$router.push({
        name: 'pagesUser-Items',
        params: { categoryId: id }
      });
    },

    async destroy (category) {
      try {
        if (confirm(`Do you wish to delete the category: id ${category.id} - ${category.type}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/categories/destroy', { id: category.id });
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

