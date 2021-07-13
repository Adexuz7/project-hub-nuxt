<template>
  <v-container>
    <v-row class="text-center">
      <v-col>
        <h1>Explore projects</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col v-for="(project, index) in projects" :key="index">
        <!-- <Project :project="project" @addlike="getProjects" /> -->
        <Project :project="project" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    return {
      projects: await $axios.$get('/projects'),
    }
  },
  methods: {
    async getProjects() {
      this.projects = await this.$axios.$get('/projects')
    },
  },
}
</script>
