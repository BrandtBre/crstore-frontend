<template>
  <v-container>
    <h1 style="">Cadastro de Metodos de Pagamentos <v-icon x-large>mdi-pencil-outline</v-icon></h1>
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
                  v-model="paymentMethods.id"
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
                  v-model="paymentMethods.type"
                  placeholder="Tipo"
                  label="Tipo"
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
          to="/paymentMethods"
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
      paymentMethods: {
        id: null,
        type: null
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
        let paymentMethods = {
          type: this.paymentMethods.type
        };
        if (!this.paymentMethods.id) {
          await this.$axios.$post('http://localhost:3333/payment-methods/persist', paymentMethods);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/payment-methods');
        }
        await this.$axios.$post(`http://localhost:3333/payment-methods/persist/${this.paymentMethods.id}`, paymentMethods);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/payment-methods');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },
    async getById (id) {
      this.paymentMethods = await this.$axios.$get(`http://localhost:3333/payment-methods/${id}`);
    }
  }
}
</script>