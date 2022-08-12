<template>
  <v-container>
    <h1 style="">Cadastro de Categorias <v-icon x-large>mdi-pencil-outline</v-icon></h1>
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
                  v-model="category.id"
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
                  v-model="category.type"
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
      category: {
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
        let category = {
          type: this.category.type
        };
        if (!this.category.id) {
          await this.$axios.$post('http://localhost:3333/categories/persist', category);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/categories');
        }
        await this.$axios.$post(`http://localhost:3333/categories/persist/${this.category.id}`, category);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/categories');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },
    async getById (id) {
      this.category = await this.$axios.$get(`http://localhost:3333/categories/${id}`);
    }
  }
}
</script>