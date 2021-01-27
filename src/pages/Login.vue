<template>
  <q-page padding>
    <div>
      <div class="q-mb-lg text-light">
        <h5 class="q-my-none q-mb-xs text-grey-10">LOGIN<span class="text-red-6 text-bold">.</span></h5>
      </div>
      <q-input class="q-mt-md" type="email" outlined v-model="email" label="Email" stack-label/>
      <q-input class="q-mt-md" type="password" outlined v-model="password" label="Password" stack-label/>
      <q-btn class="q-mt-md q-pa-xs full-width" @click="login()" color="blue" label="Login" />
      <q-btn class="q-mt-md q-pa-xs full-width" @click="redirectSignUp()" color="green" label="Don't have an account? Sign up" />
    </div>
  </q-page>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Login',
  data () {
    return {
      email: null,
      password: null
    }
  },
  methods: {
    redirectSignUp () {
      this.$router.push('/register')
    },
    login () {
      this.$q.loading.show()
      var self = this
      var data = {
        email: this.email,
        password: this.password
      }
      axios.post(process.env.API_URL + 'auth/login', data)
        .then(function (response) {
          self.$q.loading.hide()
          localStorage.setItem('token', response.data.access_token)
          self.$router.push('/')
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  }
}
</script>
