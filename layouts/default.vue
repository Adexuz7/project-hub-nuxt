<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
      color="orange lighten-2"
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
    <v-app-bar color="orange lighten-1" :clipped-left="clipped" fixed app>
      <a @click.stop="drawer = !drawer"> 
        <v-icon class="mr-5">mdi-apps</v-icon>
      </a>
      <!-- <v-app-bar-nav-icon @click.stop="drawer = !drawer" /> -->
      <v-toolbar-title v-text="title" />
      <!-- <v-spacer />
      Add things to the right side of the nav bar -->
    </v-app-bar>
    <v-main class="diferente">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer color="orange lighten-3" :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-set-center-right',
          title: 'Welcome',
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
          icon: 'mdi-account-circle',
          title: 'Profile',
          to: '/profile',
        },
      ],
      miniVariant: false,
      title: 'ProjectHub',
      
    }
  },
}
</script>

<style>
 .diferente {
  background-image: url("../assets/74173.jpg");
  background-size: 100% 100%;
  background-repeat: no-repeat;
 } 
 

</style>