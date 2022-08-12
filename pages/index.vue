<template>
 <v-container>
    <h1>Home Page</h1>
    <hr>
    <v-container>
      <p>Faça login para poder comprar em nossa loja</p>
    </v-container>
    <v-container>
        <v-row>
          <v-card
            v-for="(category, i) in categories"
              :key="i"
              router
              exact
              style="margin: 10px; border: 5px solid #e2e3ca "
          >
            <v-img
              max-width="250px"
              max-height="250px"
              src="https://cdn.pixabay.com/photo/2020/06/26/16/04/chef-5343214_960_720.png"   
            ></v-img>
            <v-row>
              <v-card-title>
                {{ category.type }}
              </v-card-title>
              <v-card-actions>
                <v-btn
                  color="green lighten-2"
                  text
                  @click="getItemsByid(category.id)"
                >Procurar</v-btn>
              </v-card-actions>
            </v-row>
          </v-card>
        </v-row>
      </v-container>
  </v-container>
</template>

<script>

export default {
  layout: 'futureDefault',
  data () {
    return {
      valid: false,
      categories: [],
      user: {
        username: null,
        password: null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },

  created () {
    this.getCategories()
  },

  methods: {
    async getCategories () {
      this.categories = await this.$axios.$get('http://localhost:3333/categories');
    },

    async getItemsByid (id) {
      this.$router.push({
        name: 'Items',
        params: { categoryId: id }
      });
    },

    async logar () {
      try {
        let user = {
          username: this.user.username,
          password: this.user.password,
        };

        let response = await this.$api.post('/users/login', user);

        this.$toast.success(response.message);
        
        localStorage.setItem('crstore-api-token', response.token);
        
      } catch (error) {
        console.log(error)
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },

  }
}
</script>
