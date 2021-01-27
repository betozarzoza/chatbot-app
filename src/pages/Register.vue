<template>
  <q-page padding>
    <div>
      <div class="q-mb-lg text-light">
        <h5 class="q-my-none q-mb-xs text-grey-10">REGISTER<span class="text-red-6 text-bold">.</span></h5>
      </div>
      <q-input class="q-mt-md" type="text" outlined v-model="name" label="Name" stack-label/>
      <q-input class="q-mt-md" type="email" outlined v-model="email" label="Email" stack-label/>
      <q-input class="q-mt-md" type="password" outlined v-model="password" label="Password" stack-label/>
      <q-input class="q-mt-md" type="password" outlined v-model="password_confirmation" label="Confirm password" stack-label/>
      <q-btn class="q-mt-md q-pa-xs full-width" @click="register()" color="blue" label="Register" />
      <q-btn class="q-mt-md q-pa-xs full-width" @click="redirectLogin()" color="green" label="Login" />
    </div>
  </q-page>
</template>

<script>

import axios from 'axios'
export default {
  name: 'Register',
  data () {
    return {
      name: null,
      email: null,
      password: null,
      password_confirmation: null
    }
  },
  methods: {
    redirectLogin () {
      this.$router.push('/login')
    },
    register () {
      this.$q.loading.show()
      var self = this
      var data = {
        name: this.name,
        email: this.email,
        password: this.password,
        password_confirmation: this.password_confirmation
      }
      axios.post(process.env.API_URL + 'auth/signup', data)
        .then(function (response) {
          self.$q.notify({
            type: 'positive',
            message: 'Usuario creado.'
          })
          var dataLogin = {
            email: self.email,
            password: self.password
          }
          axios.post(process.env.API_URL + 'auth/login', dataLogin)
            .then(function (response) {
              self.$q.loading.hide()
              localStorage.setItem('token', response.data.access_token)
              self.$router.push('/')
            })
            .catch(function (error) {
              self.errorHandling(error)
            })
        })
        .catch(function (error) {
          self.errorHandling(error)
        })
    },
    errorHandling (err) {
      var vthis = this
      this.$q.loading.hide()
      const error = err.response.data
      console.log(error)
      if (err.response.request.status === 401) {
        vthis.$q.dialog({
          title: 'Error',
          message: 'Usuario o contraseña incorrectos, ¿Desea reintentar?',
          persistent: true
        })
      }
      if (error.message === 'Network Error') {
        vthis.$q.dialog({
          title: 'Error',
          message: 'No hay conexion al servidor, ¿Desea reintentar?',
          persistent: true
        }).onOk(() => {
          location.reload()
        })
      }
      //  error = JSON.parse(error.request.response)
      //  error = JSON.parse(error)
      if (error.errors) {
        console.log('error.errors')
        if (error.errors.email) {
          vthis.$q.dialog({
            title: 'Error',
            message: error.errors.email[0],
            persistent: true
          })
        }
        if (error.errors.name) {
          vthis.$q.dialog({
            title: 'Error',
            message: error.errors.name[0],
            persistent: true
          })
        }
        if (error.errors.password) {
          vthis.$q.dialog({
            title: 'Error',
            message: error.errors.password[0],
            persistent: true
          })
        }
        if (error.errors.last_name) {
          vthis.$q.dialog({
            title: 'Error',
            message: error.errors.last_name[0],
            persistent: true
          })
        }
        if (error.errors.phone) {
          vthis.$q.dialog({
            title: 'Error',
            message: error.errors.phone[0],
            persistent: true
          })
        }
      }
    }
  }
}
</script>
