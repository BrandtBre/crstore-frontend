<template>
  <v-container>
    <h1 style="">Cadastro de Produtos/Items<v-icon x-large>mdi-pencil-outline</v-icon></h1>
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
                  v-model="item.id"
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
                  v-model="item.name"
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
                  v-model="item.price"
                  placeholder="Preço"
                  label="Preço"
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
                  v-model="item.categoryId"
                  placeholder="Id Da Categoria"
                  label="Id Da Categoria"
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
          to="/items"
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
      item: {
        id: null,
        name: null,
        price: null,
        categoryId: null,
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
        let item = {
          name: this.item.name,
          price: this.item.price,
          categoryId: this.item.categoryId
        };
        if (!this.item.id) {
          await this.$api.post('/items/persist', item);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/Items');
        }
        await this.$api.post(`/items/persist/${this.item.id}`, item);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/Items');
      } catch (error) {
        console.log(error)
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },
    async getById (id) {
      this.item = await this.$api.$get(`http://localhost:3333/items/${id}`);
    }
  }
}
</script>