<template>
  <v-container>
    <h1>Consulta de Endereços</h1>
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
            to="/Addresses/cadastro"
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
        :items="addresses"
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
      layout: "userLayout",
      headers: [
        {
          text: 'Código', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'id', //é o dado que essa coluna vai receber
        },
        {
          text: 'Rua',
          align: 'center',
          sortable: false,
          value: 'street',
        },
        {
          text: 'Bairro',
          align: 'center',
          sortable: false,
          value: 'district',
        },
        {
          text: 'Numero',
          align: 'center',
          sortable: false,
          value: 'number',
        },
        {
          text: 'Cidade',
          align: 'center',
          sortable: false,
          value: 'city',
        },
        
        {text: "", value: "actions"}
      ],
      addresses: []
    }
  },

  created () {
    this.getAdressByUserId();
  },
  
  methods: {
    async getAddresses () {
      let response = await this.$api.get('/adresses');
      this.addresses = response.data;
    },

    async getAdressByUserId () {
      try {
        let response = await this.$api.get('/adresses/user');
        this.addresses = response.data;       
      } catch (error) {
        this.$toast.error('Não foi possível buscas os seus endereços, mals ae =( ');
      }
    },

    async destroy (address) {
      try {
        if (confirm(`Quer deleta o endereço: id ${address.id} ?`)) {
          let response = await this.$api.post('http://localhost:3333/adresses/destroy', { id: address.id });
          this.$toast.success(response.message)
          this.getAdressByUserId();
        }
      } catch (error) {
        this.$toast.error('An error has ocurred while trying to delete the category =( ');
      }
    },

    async editItem (address) {
      console.log(address)
      this.$router.push({
        name: 'Addresses-cadastro',
        params: {
          id: address.id
        }
      })
    }
  }
}
</script>