<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          Chatbot app
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          Menu
        </q-item-label>
        <q-item
          clickable
          @click="redirectWhatsapp()"
        >
          <q-item-section
            avatar
          >
            <q-icon name="favorite" />
          </q-item-section>

          <q-item-section>
            <q-item-label>Created by Beto Zarzoza</q-item-label>
            <q-item-label caption>
              Send a message by whatsapp
            </q-item-label>
          </q-item-section>
        </q-item>
        <q-item
          clickable
          @click="logout()"
        >
          <q-item-section
            avatar
          >
            <q-icon name="lock" />
          </q-item-section>

          <q-item-section>
            <q-item-label>Logout</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>

export default {
  name: 'MainLayout',
  data () {
    return {
      leftDrawerOpen: false
    }
  },
  methods: {
    logout () {
      localStorage.removeItem('token')
      this.$router.push('/login')
    },
    redirectWhatsapp () {
      window.location.href = 'https://wa.me/+528120392234'
    }
  }
}
</script>
