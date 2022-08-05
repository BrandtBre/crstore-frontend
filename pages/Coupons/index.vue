<template>
  <v-container>
    <h1>Consulta De Cupons</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
            @click="getCoupons"
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
            to="/coupons/cadastro"
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
        :items="coupons"
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
  data () {
    return {
      headers: [
        {
          text: 'Codigo', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'code', //é o dado que essa coluna vai receber
        },
        {
          text: 'Data de Limite',
          align: 'center',
          sortable: false,
          value: 'limitDate',
        },
        {
          text: 'Categoria',
          align: 'center',
          sortable: false,
          value: 'discountPercentage',
        },
        
        {text: "", value: "actions"}
      ],
      coupons: []
    }
  },
  created () {
    this.getCoupons()
  },
  
  methods: {
    async getCoupons () {
      this.coupons = await this.$axios.$get('http://localhost:3333/coupons');
    },
    async destroy (category) {
      try {
        if (confirm(`Do you wish to delete the category: id ${category.id} - ${category.type}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/coupons/destroy', { id: category.id });
          this.$toast(response.message)
          this.getCoupons();
        }
      } catch (error) {
        this.$toast.error('An error has ocurred while trying to delete the category =(');
      }
    },
    async editItem (category) {
      this.$router.push({
        name: 'Coupons-cadastro',
        params: { id: category.id }
      });
    }
  }
}
</script>