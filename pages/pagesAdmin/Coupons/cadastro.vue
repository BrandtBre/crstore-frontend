<template>
  <v-container>
    <h1 style="">Cadastro de Coupons <v-icon x-large>mdi-pencil-outline</v-icon></h1>
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
                  v-model="coupon.id"
                  placeholder="ID"
                  label="ID"
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
                  v-model="coupon.code"
                  placeholder="Codigo do Cupom"
                  label="Codigo do Cupom"
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
                <v-date-picker
                  v-model="coupon.limitDate"
                  required
                  outlined
                  :rules="rule"
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col
                cols="7"
              >
                <v-text-field
                  v-model="coupon.discountPercentage"
                  placeholder="Porcentagem de Desconto"
                  label="Porcentagem de Desconto"
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
          to="/coupons"
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
  layout: 'originalDefault',
  name: 'CadastroAutoresPage',
  data () {
    return {
      valid: false,
      coupon: {
        id: null,
        code: null,
        limitDate: null,
        discountPercentage: null,
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
        let coupon = {
          code: this.coupon.code,
          limitDate: this.coupon.limitDate,
          discountPercentage: this.coupon.discountPercentage,
        };
        if (!this.coupon.id) {
          await this.$axios.$post('http://localhost:3333/coupons/persist', coupon);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/coupons');
        }
        await this.$axios.$post(`http://localhost:3333/coupons/persist/${this.coupon.id}`, coupon);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/coupons');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },
    async getById (id) {
      let response = await this.$axios.$get(`http://localhost:3333/coupons/${id}`);
      this.coupon = response.data;
    }
  }
}
</script>