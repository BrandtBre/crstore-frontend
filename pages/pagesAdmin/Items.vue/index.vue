<template>
  <v-container>
    <h1>Consulta De Produtos</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
            @click="getItems()"
          >
            Pesquisar
            <v-icon style="margin-left: 5%">
              mdi-magnify
            </v-icon>
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="margin-left: -77%"
            outlined
            to="/items/cadastro"
          >
            Cadastrar
            <v-icon style="margin-left: 5%">
              mdi-plus-circle-outline
            </v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="items"
        :items-per-page="10"
        class="elevation-1"
      >
        <template v-slot:item.actions="{ item }">
              <v-icon
                small
                class="mr-2"
                @click="editItem(item)"
              >
                mdi-pencil
              </v-icon>
              <v-icon
                small
                @click="destroy(item)"
              >
                mdi-delete
              </v-icon>
        </template>
      </v-data-table>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaAutoresPage',
  layout: 'originalDefault',
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
      items: []
    }
  },
  created () {
    this.getItems()
  },
  
  methods: {
    async getItems () {
      let response = await this.$axios.$get('http://localhost:3333/items');
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