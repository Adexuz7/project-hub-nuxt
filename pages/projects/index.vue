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
        @projectCreated="getProjects"
      />
    </v-row>
    <!-- <v-row>
      <v-col>
        <NewProject v-if="isAuthenticated" :select-categories="categories" @projectCreated="getProjects"></NewProject>
      </v-col>
    </v-row> -->
    <v-row>
      <v-col v-for="(project, index) in projects" :key="index">
        <!-- <Project :project="project" @addlike="getProjects" /> -->
        <Project :project="project" @addlike="addLike" />
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
    async getProjects() {
      this.projects = await this.$axios.$get('/projects')
    },
    addLike(project) {
      const arr = this.projects.map((project) => project._id)

      this.projects.splice(arr.indexOf(project._id), 1, project)
    },
    toLogin() {
      this.router.push('/login')
    }
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Satisfy&display=swap');

h1 {
display: flex;
justify-content: center;
font-family: 'Satisfy', 'Montserrat', sans-serif;
letter-spacing: 10px;
}
</style>