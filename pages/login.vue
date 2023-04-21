<template>
  <v-container fluid fill-height>
    <v-layout align-center justify-center>
      <v-flex xs12 sm8 md4>
        <v-form fast-fail @submit.prevent="login">
          <v-card class="elevation-12">
            <v-toolbar dark color="primary">
              <v-toolbar-title>Login form</v-toolbar-title>
            </v-toolbar>
            <v-card-text>
              <v-text-field
                v-model="userName"
                prepend-icon="mdi-account"
                name="login"
                label="Login"
                type="text"
                :rules="userNameRules"
              ></v-text-field>
              <v-text-field
                v-model="password"
                id="password"
                prepend-icon="mdi-lock"
                name="password"
                label="Password"
                type="password"
                :rules="passwordRules"
              ></v-text-field>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" type="submit" :loading="loader" 
                >Login</v-btn
              >
            </v-card-actions>
          </v-card>
        </v-form>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  name: 'Login',
  layout: 'blank',
  data() {
    return {
      loader: false,
      userName: '',
      userNameRules: [
        (value) => {
          if (value) return true
          return 'User name required.'
        },
      ],
      password: '',
      passwordRules: [
        (value) => {
          if (value) return true
          return 'Password name required.'
        },
      ],
    }
  },
  props: {
    source: String,
  },
  beforeMount() {
    this.checkAuth()
  },
  methods: {
    checkAuth() {
      if (localStorage.getItem('token')) {
        this.$router.push('/')
      }
    },
    async login() {
      this.loader = true
      const payload = {
        email: this.userName,
        password: this.password,
        rememberMe: true,
      }
      try {
        const { data } = await this.$nuxt.$axios.post('login', payload)
        this.$nuxt.$axios.defaults.headers.Authorization = data.token
        await localStorage.setItem('token', data.token)
        this.checkAuth()
      } catch (error) {
        debugger
      } finally {
        this.loader = false
      }
    },
  },
}
</script>

<style></style>
