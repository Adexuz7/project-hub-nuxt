<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Explore projects</h1>
        <hr />
      </v-col>
    </v-row>
    <v-row>
      <NewProject
        v-if="isAuthenticated"
        :select-categories="categories"
        @projectCreated="refresh"
      />
    </v-row>
    <v-row>
      <v-col
        v-for="(project, index) in projects.slice().reverse()"
        :key="index"
      >
        <Project :project="project" :post-it="true" @addlike="refresh" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  auth: false,
  async asyncData({ $axios }) {
    return {
      projects: await $axios.$get('/projects'),
      categories: await $axios.$get('/categories'),
    }
  },
  computed: {
    ...mapGetters(['isAuthenticated']),
  },
  methods: {
    async refresh() {
      this.projects = await this.$axios.$get('/projects')
    },
    toLogin() {
      this.router.push('/login')
    },
  },
}
</script>

<style scoped>
h1 {
  display: flex;
  justify-content: center;
  font-family: 'Montserrat', sans-serif;
  letter-spacing: 10px;
}
</style>