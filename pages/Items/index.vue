<template>
  <v-container>
    <v-container>
      <v-container style="text-align: center">
        <h1>Produtos</h1>
        <hr>
      </v-container>
      <v-container>
        <v-row>
          <v-card
            v-for="(item, i) in items"
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
              <v-col>
                <v-card-title>
                  {{ item.name }}
                </v-card-title> 
              </v-col>            
            </v-row>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col>
                    <div >
                      $ • {{ item.price }}
                    </div>
                  </v-col>
                </v-row>
                <v-row style="margin-left: -20%">
                  <v-col>
                    <v-card-actions>
                      <v-btn
                        color="yellow lighten-2"
                        text
                        outlined
                        @click="addToCart(item)"
                      >
                        Comprar
                      </v-btn>
                    </v-card-actions>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
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
          text: 'Nome',
          align: 'center',
          sortable: false,
          value: 'name',
        },
        {
          text: 'Preço',
          align: 'center',
          sortable: false,
          value: 'price',
        },
        {
          text: 'Id da Categoria',
          align: 'center',
          sortable: false,
          value: 'categoryId',
        },
        {text: "", value: "actions"}
      ],
      categories: [],
      items: [],
      cart: {
        id: null,
        items: null,
        userId: null,
      }
    }
  },
  created () {
    if (this.$route?.params?.categoryId) {
      return this.getItems(this.$route.params.categoryId)
    } 
    this.getAllItems();
  },
  
  methods: {
    async getItems (categoryId) {
      let response = await this.$api.get(`/items/category/${categoryId}`);
      this.items = response.data;
    },

    async addToCart (item) {
      let cart = {
        items: item
      };

      if(confirm(`Quer adicionar esse produto no carrinho ?`)){
        await this.$api.post('/carts/persist', cart);
        this.$toast.success(`Produto ${item.name} adicionado ao carrinho com sucesso`);
      }
    },

    async getAllItems () {
      let response = await this.$api.get('/items/');
      this.items = response.data;
    },

    async destroy (product) {
      try {
        if (confirm(`Do you wish to delete the product: id ${product.id} ?`)) {
          let response = await this.$axios.$post('http://localhost:3333/items/destroy', { id: product.id });
          this.$toast(response.message)
          this.getItems();
        }
      } catch (error) {
        this.$toast.error('An error has ocurred while trying to delete the product =(');
      }
    },
    async editItem (product) {
      this.$router.push({
        name: 'Items-cadastro',
        params: { id: product.id }
      });
    }
  }
}

</script>
