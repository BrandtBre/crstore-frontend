<template>
  <v-container>
    <h1 style="">Registro<v-icon x-large>mdi-pencil-outline</v-icon></h1>
    <hr>
    <v-container style="border: 3px solid ; margin-top: 100px; border-radius:10px" >
      <v-container >
        <v-form v-model="valid">
          <v-container>
            <v-row>
              <v-col
                cols="7"
              >
                <v-text-field
                  v-model="user.username"
                  placeholder="Nome de Usuario"
                  label="Nome de Usuario"
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
                  v-model="user.cpf"
                  placeholder="cpf"
                  label="cpf"
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
                  v-model="user.name"
                  placeholder="Nome"
                  label="Nome"
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
                  v-model="user.phone"
                  placeholder="Telefone"
                  label="Telefone"
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
                  v-model="user.role"
                  placeholder="Ocupação"
                  label="Ocupação"
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
          to="/categories"
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
      user: {
        id: null,
        username: null,
        cpf: null,
        name: null,
        phone: null,
        role: null,
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },
  
  methods: {
    async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let user = {
          username: this.user.username,
          cpf: this.user.cpf,
          name: this.user.name,
          phone: this.user.phone,
          role: this.user.role,
        };
        
        await this.$api.post('/users/register', user);
        this.$toast.success('Cadastro realizado com sucesso!');
        return this.$router.push('/categories');
        
      } catch (error) {   
        console.log(error)
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },
    async getById (id) {
      this.user = await this.$api.$get(`http://localhost:3333/adresses/${id}`);
    }
  }
}
</script>