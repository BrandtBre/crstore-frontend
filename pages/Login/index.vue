<template>
 <v-container>
    <v-container style="margin-top: 200px">
      <v-row>
        <v-col>
          <h1><v-icon style="color:#FFC72C;" x-large>mdi-food-fork-drink</v-icon>CRStore</h1>
        </v-col>
      </v-row>
    </v-container>
    <v-container style="margin-top: 60px">
      <v-row>
        <v-col>
          <h1 style="font-size: 55px">Faça seu login</h1>
          <h1 style="font-size: 55px">na plataforma</h1>
        </v-col>
        <v-col>
          <v-container style="padding:20%; background-color:#FFC72C; border-radius:5px; margin-top: -200px;">
            <form action="">
              <v-text-field
                outlined
                v-model="user.username"
                label="Nome de Usuario"
                prepend-inner-icon="mdi-account-circle-outline"
                color="#DA291C"
                background-color="white"      
                :rules="rule"
                required         
              >
              </v-text-field>           
              <v-text-field
                outlined
                v-model="user.password"
                label="Senha"
                prepend-inner-icon="mdi-lock"
                color="#DA291C"
                background-color="white"
                type="password"
                :rules="rule"
                required                  
              ></v-text-field>
              <v-col style="margin-top: -20px;">
                <a href="forgotPassword" style="font-weight:bold; color:#DA291C; font-size: 15px;">
                Esqueci minha senha
                </a>
              </v-col>        
              <v-btn style="background-color:#DA291C; width: 100% ; margin-top: 15px; padding: 7%" @click="logar">ENTRAR</v-btn>
              <p style="text-align: center; margin-top: 20px;">Não tem uma conta? <a style="font-weight:bold; color:#DA291C; ">Registre-se</a></p>
              <v-row>
                <v-col>
                  <hr>
                </v-col>
                <v-col>
                  <hr>
                </v-col>
              </v-row>
            </form>
          </v-container>
        </v-col>
      </v-row>
    </v-container>
  </v-container>
</template>

<script>

export default {
  layout: 'adminLayout',
  data () {
    return {
      valid: false,
      user: {
        username: null,
        password: null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },

  methods: {
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
