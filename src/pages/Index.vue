<template>
  <q-page class="q-pa-md row justify-center">
    <div style="width: 100%; max-width: 400px">
      <q-chat-message
        v-for="(message, index) in messages" :key="index"
        :text="message.message"
        :avatar="message.avatar"
        :sent="message.sent"
      />
    </div>
    <div class="full-width absolute-bottom">
      <q-input v-on:keyup.enter="sendMessage" filled v-model="message" @enter="sendMessage">
        <template v-slot:append>
          <q-btn round dense flat icon="send" @click="sendMessage()" />
        </template>
      </q-input>
    </div>
  </q-page>
</template>

<script>
import axios from 'axios'

export default {
  name: 'PageIndex',
  data () {
    return {
      message: null,
      messages: [
        {
          message: ['Hello! welcome to the bot'],
          sent: true,
          avatar: require('assets/botpp.png')
        }
      ]
    }
  },
  mounted () {
    this.messages.push(
      {
        message: ['/balance', '/add {amount}', '/withdraw {amount}', '/exchange {amount} {from} {to}', '/help'],
        sent: true,
        avatar: require('assets/botpp.png')
      }
    )
  },
  methods: {
    sendMessage () {
      const self = this
      self.createUserMessage(this.message)
      const message = this.message.split(' ')
      let data
      const command = message[0]
      const config = {
        headers: { Authorization: 'Bearer ' + localStorage.getItem('token') }
      }
      switch (command) {
        case '/balance':
          data = {
            command: '/balance'
          }
          axios.post(process.env.API_URL + 'messages/receive', data, config)
            .then(function (response) {
              self.createBotMessage('Your balance is ' + response.data.balance)
            })
          break
        case '/add':
          data = {
            command: '/add',
            amount: parseFloat(message[1])
          }
          axios.post(process.env.API_URL + 'messages/receive', data, config)
            .then(function (response) {
              self.createBotMessage('Your balance is ' + response.data.balance)
            })
          break
        case '/withdraw':
          data = {
            command: '/withdraw',
            amount: parseFloat(message[1])
          }
          axios.post(process.env.API_URL + 'messages/receive', data, config)
            .then(function (response) {
              self.createBotMessage('Your balance is ' + response.data.balance)
            }).catch(function (error) {
              console.log(error.response)
              self.createBotMessage(error.response.data.message)
            })
          break
        case '/exchange':
          data = {
            command: '/exchange',
            amount: parseFloat(message[1]),
            from_currency: message[2],
            to_currency: message[3]
          }
          axios.post(process.env.API_URL + 'messages/receive', data, config)
            .then(function (response) {
              console.log(response)
              self.createBotMessage(parseFloat(message[1]) + ' ' + message[2] + ' are ' + response.data.amount + ' ' + message[3])
            })
          break
        case '/help':
          self.showCommands()
          break
        default:
          self.createBotMessage(command + 'is an invalid command.')
          break
      }
      this.message = null
    },
    createBotMessage (message) {
      this.messages.push(
        {
          message: [message],
          sent: true,
          avatar: require('assets/botpp.png')
        }
      )
    },
    createUserMessage (message) {
      this.messages.push(
        {
          message: [message],
          sent: false,
          avatar: 'https://cdn.quasar.dev/img/avatar2.jpg'
        }
      )
    },
    showCommands () {
      this.messages.push(
        {
          message: ['/balance', '/add {amount}', '/withdraw {amount}', '/exchange {amount} {from} {to}', '/help'],
          sent: true,
          avatar: require('assets/botpp.png')
        }
      )
    }
  }
}
</script>
