<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Explore projects</h1>
        <hr>
      </v-col>
    </v-row>
    <v-row>
      <v-col v-for="(project, index) in projects" :key="index">
        <!-- <Project :project="project" @addlike="getProjects" /> -->
        <Project :project="project" @addlike="addLike"/>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  auth: false,
  async asyncData({ $axios }) {
    return {
      projects: await $axios.$get('/projects'),
    }
  },
  methods: {
    async getProjects() {
      this.projects = await this.$axios.$get('/projects')
    },
    addLike(project) {
      const arr = this.projects.map(project => project._id)

      this.projects.splice(arr.indexOf(project._id), 1, project)
    }
  },
}
</script>

<style scoped>
h1 {
display: flex;
justify-content: center;
font-family: 'Montserrat Alternates', sans-serif;
letter-spacing: 10px;
text-shadow: 1px 1px 2px black;
}
</style>