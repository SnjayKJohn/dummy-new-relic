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
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn v-show="drawer" icon @click.stop="miniVariant = !miniVariant">
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn v-show="drawer" icon @click.stop="clipped = !clipped">
        <v-icon>mdi-application</v-icon>
      </v-btn>
      
      <v-toolbar-title>{{ title }}</v-toolbar-title>
      <v-spacer />
      <v-btn icon @click.stop="rightDrawer = !rightDrawer">
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer v-model="rightDrawer" :right="right" temporary fixed>
      <v-list dense>
         <v-list-item-group>
        <v-list-item @click.native="switchTheme">
          <v-list-item-action>
            <v-icon > mdi-theme-light-dark </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch theme</v-list-item-title>
        </v-list-item>
        <v-list-item @click.native="logout">
          <v-list-item-action>
            <v-icon > mdi-power </v-icon>
          </v-list-item-action>
          <v-list-item-title>Logout</v-list-item-title>
        </v-list-item>
         </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire',
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'JUMiNGO',
    }
  },
  beforeMount() {
    const token = localStorage.getItem('token')
    if (token) {
      this.$nuxt.$axios.defaults.headers.Authorization = `Bearer ${token}`
    } else {
      this.$router.push('login')
    }
  },
  methods: {
    logout() {
      localStorage.setItem('token', '')
      this.$router.push('login')
    },
    switchTheme(){
      this.$nuxt.$vuetify.theme.isDark = !this.$nuxt.$vuetify.theme.isDark;
    }
  },
}
</script>
