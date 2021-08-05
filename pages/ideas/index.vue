<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Explore ideas</h1>
        <hr />
      </v-col>
    </v-row>
    <v-row>
      <NewIdea
        v-if="isAuthenticated"
        :select-categories="categories"
        @ideaCreated="refresh"
      />
    </v-row>
    <v-row align="center">
      <v-col v-for="(idea, index) in ideas.slice().reverse()" :key="index">
        <Idea v-if="isAuthenticated" :idea="idea" :post-it="true" @like="refresh" />
        <Idea v-else :idea="idea" :post-it="true" @like="toLogin" />
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
      ideas: await $axios.$get('/ideas'),
      categories: await $axios.$get('/categories'),
    }
  },
  computed: {
    ...mapGetters(['isAuthenticated']),
  },
  methods: {
    async refresh() {
      this.ideas = await this.$axios.$get(`/ideas`)
    },
    toLogin() {
      this.rooter.push('/login')
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Satisfy&display=swap');

h1 {
  display: flex;
  justify-content: center;
  font-family: 'Montserrat', sans-serif;
  letter-spacing: 10px;
}
</style>
