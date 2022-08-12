<template>
  <v-container>
    <h1>Consulta de Pedidos</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
            @click="getCategories()"
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
            to="/Orders/cadastro"
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
        :items="orders"
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
          value: 'id', //é o dado que essa coluna vai receber
        },
        {
          text: 'Status',
          align: 'center',
          sortable: false,
          value: 'status',
        },
        {
          text: 'Customer',
          align: 'center',
          sortable: false,
          value: 'customerId',
        },
        {
          text: 'Funcionário',
          align: 'center',
          sortable: false,
          value: 'employeeId',
        },
        {
          text: 'Metodo de Pagamento',
          align: 'center',
          sortable: false,
          value: 'paymentMethodId',
        },
        {
          text: 'Coupon',
          align: 'center',
          sortable: false,
          value: 'couponId',
        },
        
        {text: "", value: "actions"}
      ],
      orders: []
    }
  },
  created () {
    this.getOrders()
  },
  
  methods: {
    async getOrders () {
      let response = await this.$api.$get('http://localhost:3333/orders');
      this.orders = response.data;
    },
    async destroy (order) {
      try {
        if (confirm(`Do you wish to delete the order: id ${order.id} ?`)) {
          let response = await this.$api.$post('http://localhost:3333/orders/destroy', { id: order.id });
          this.$toast(response.message)
          this.getOrders(); 
        }
      } catch (error) {
        this.$toast.error('An error has ocurred while trying to delete the order =( ');
      }
    },
    async editItem (order) {
      this.$router.push({
        name: 'Orders-cadastro',
        params: { id: order.id }
      });
    }
  }
}
</script>