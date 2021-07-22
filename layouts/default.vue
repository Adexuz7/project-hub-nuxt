<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      app
      mobile-breakpoint="200000"
    >
      <v-list v-if="loggedInUser">
        <v-list-item nuxt to="/profile">
          <v-list-item-content>
            <v-list-item-title class="text-h6">
              {{ loggedInUser.name }}
            </v-list-item-title>
            <v-list-item-subtitle>{{
              loggedInUser.email
            }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>

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
        <v-list-item-content >
          <v-list-item-title v-text="item.title" />
        </v-list-item-content>
      </v-list-item>
    </v-navigation-drawer>
    <v-app-bar color="yellow" flat :clipped-left="clipped" app>
      <!-- <v-app-bar-nav-icon @click.stop="drawer = !drawer" /> -->
      <v-btn large text @click.stop="drawer = !drawer">
        <v-icon>mdi-menu</v-icon>
      </v-btn>
      <v-spacer />
      <nuxt-link class="title" to="/">
        <v-toolbar-title v-text="title" />
      </nuxt-link>
      <v-spacer />
      <v-btn class="btn-profile" large text to="/profile">
        <v-icon>mdi-account</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer absolute app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  middleware: 'auth',
  data() {
    return {
      clipped: true,
      drawer: false,
      items: [
        {
          icon: 'mdi-home',
          title: 'Home',
          to: '/',
        },
        {
          icon: 'mdi-lightbulb-on',
          title: 'Ideas',
          to: '/ideas',
        },
        {
          icon: 'mdi-file',
          title: 'Projects',
          to: '/projects',
        },
        {
          icon:'mdi-account-group',
          title:'Teams',
          to:'/teams'
        },
      ],
      miniVariant: false,
      title: 'ProjectHub',
    }
  },
  computed: {
    ...mapGetters(['loggedInUser']),
  },
}
</script>

<style scoped>
.btn-profile.v-btn--active::before {
  opacity: 0;
}
.title {
  color: black;
  text-decoration: none;
  font-weight: normal;
}
</style>