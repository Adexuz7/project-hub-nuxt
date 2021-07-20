<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      app
      mobile-breakpoint="200000"
      color="pink lighten-4"
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
    <v-app-bar color="pink lighten-2" :clipped-left="clipped" app>
      <!-- <v-app-bar-nav-icon @click.stop="drawer = !drawer" /> -->
      <v-btn large rounded text @click.stop="drawer = !drawer">
        <v-icon>mdi-set-center-right</v-icon>
      </v-btn>
      <v-spacer />
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn class="btn-profile" large rounded text to="/profile">
        <v-icon>mdi-account</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main class="bg-color pink lighten-4">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer color="pink lighten-3" absolute app>
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
</style>