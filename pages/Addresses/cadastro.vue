<template>
  <v-container>
    <h1 style="">Cadastro de Endereços<v-icon x-large>mdi-pencil-outline</v-icon></h1>
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
                  v-model="address.id"
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
                  v-model="address.street"
                  placeholder="Rua"
                  label="Rua"
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
                  v-model="address.district"
                  placeholder="Bairro"
                  label="Bairro"
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
                  v-model="address.number"
                  placeholder="Numero"
                  label="Numero"
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
                  v-model="address.city"
                  placeholder="Cidade"
                  label="Cidade"
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
                  v-model="address.complement"
                  placeholder="Complemento"
                  label="Complemento"
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
                  v-model="address.userId"
                  placeholder="Id do Usuario"
                  label="Id do Usuario"
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
          to="/addresses"
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
      address: {
        id: null,
        street: null,
        district: null,
        number: null,
        city: null,
        complement: null,
        userId: null
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
        let address = {
          street: this.address.street,
          district: this.address.district,
          number: this.address.number,
          city: this.address.city,
          complement: this.address.complement,
          userId: this.address.userId
        };
        if (!this.address.id) {
          await this.$api.post('/adresses/persist', address);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/Addresses');
        }
        await this.$api.post(`/adresses/persist/${this.address.id}`, address);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/Addresses');
      } catch (error) {
        console.log(error)
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },
    async getById (id) {
      this.address = await this.$api.$get(`http://localhost:3333/adresses/${id}`);
    }
  }
}
</script>