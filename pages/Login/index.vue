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
                v-model="Email"
                label="Email"
                prepend-inner-icon="mdi-email"
                color="#DA291C"
                background-color="white"               
              >
              </v-text-field>           
              <v-text-field
                outlined
                v-model="Senha"
                label="Senha"
                prepend-inner-icon="mdi-lock"
                color="#DA291C"
                background-color="white"

                
              ></v-text-field>
              <v-col style="margin-top: -38px">
                <a href="forgotPassword" style="font-weight:bold; color:#DA291C; font-size: 15px;">
                Esqueci minha senha
                </a>
              </v-col>        
              <v-btn style="background-color:#DA291C; width: 100% ; margin-top: 15px; padding: 7%">ENTRAR</v-btn>
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
  data () {
    return {
      user: {
        id: null, 
        cpf: null,
        username: null,
        name: null,
        phone: null,

      }
    }
  },

  methods: {
    async logar () {
      try {
        
      } catch (error) {
        
      }
    }
  }
}
</script>
