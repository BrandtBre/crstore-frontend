<template>
  <v-container>
    <h1 style="">Cadastro de Pedido<v-icon x-large>mdi-pencil-outline</v-icon></h1>
    <hr>
    <v-container style="border: 3px solid ; margin-top: 100px; border-radius:10px" >
      <v-container >
        <v-form v-model="valid">
          <v-container>
            <v-row> 
              <v-col
                cols="2"           
              >
                <v-text-field
                  v-model="order.id"
                  placeholder="Código"
                  label="Código"
                  disabled
                  outlined
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col
                cols="7"
              >
                <v-text-field
                  v-model="order.userId"
                  placeholder="Id do Usuario"
                  label="Id do Usuario"
                  :rules="rule"
                  required
                  outlined
                  color="#FFC72C"
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col
                cols="7"
              >
                <v-text-field
                  v-model="order.employeeId"
                  placeholder="Id do Funcionario"
                  label="Id do Funcionario"
                  :rules="rule"
                  required
                  outlined
                  color="#FFC72C"
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col
                cols="7"
              >
                <v-text-field
                  v-model="order.paymentMethodId"
                  placeholder="metodo de pagamento"
                  label="metodo de pagamento"
                  :rules="rule"
                  required
                  outlined
                  color="#FFC72C"
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col
                cols="7"
              >
                <v-text-field
                  v-model="order.couponId"
                  placeholder="Coupon"
                  label="Coupon"
                  :rules="rule"
                  required
                  outlined
                  color="#FFC72C"
                />
              </v-col>
            </v-row>
          </v-container>
        </v-form>
      </v-container >
      <v-container >
        <v-btn
          outlined
          to="/orderes"
          color="red"
        >
          Cancelar
        </v-btn>
        <v-btn
          outlined
          @click="cadastrar()"
          color="green"
        >
          Cadastrar
        </v-btn>
      </v-container>
    </v-container>
  </v-container>
</template>

<script>
export default {
  layout: 'adminLayout',
  name: 'CadastroAutoresPage',
  data () {
    return {
      valid: false,
      order: {
        id: null,
        status: null,
        customerId: null,
        employeeId: null,
        paymentMethodId: null,
        couponId: null       
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },
  created () {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },
  methods: {
    async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let order = {
          status: this.order.status,
          customerId: this.order.customerId,
          employeeId: this.order.employeeId,
          paymentMethodId: this.order.paymentMethodId,
          couponId: this.order.couponId,
        };
        if (!this.order.id) {
          await this.$api.post('/orders/persist', order);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/Orders');
        }
        await this.$api.post(`/orders/persist/${this.order.id}`, order);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/Orders');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },

    async getById (id) {
      let response = await this.$api.$get(`http://localhost:3333/orders/${id}`);
      this.order = response.data;
    }
  }
}
</script>