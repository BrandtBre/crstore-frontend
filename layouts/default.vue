<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-application</v-icon>
      </v-btn>
      
      <v-toolbar-title v-text="title" style=" color: red;"/>
      <v-spacer />
      <v-btn
        icon
        @click.stop="rightDrawer = !rightDrawer"
      >
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
    >
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light>
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-home',
          title: 'Ol??',
          to: '/pagesUser'
        },
        {
          icon: 'mdi-account',
          title: 'Logout',
          to: '/pagesUser/lougOut'
        },   
        {
          icon: 'mdi-food',
          title: 'Categorias',
          to: '/pagesUser/categories'
        },    
        {
          icon: 'mdi-food-turkey',
          title: 'Produtos',
          to: '/pagesUser/items'
        },
        {
          icon: 'mdi-map-marker-outline',
          title: 'Meus Endere??os',
          to: '/pagesUser/addresses'
        },  
        {
          icon: 'mdi-cart-outline',
          title: 'Carrinho',
          to: '/pagesUser/carts'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'CRStore'
    }
  },

  created () {

  },

  methods: {

    async validacao () {
      const token = localStorage.getItem('crstore-api-token') || '';

      let response = await this.$api.get('/users/validate-token', {
        headers: {Authorization: `Bearer ${token}`}
      });

      if (response.type !== 'success') {
        this.$toast.info('Voc?? n??o tem permiss??o para acessar esse recurso');
      }
      
    }


  }

}
</script>
