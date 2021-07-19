<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Explore ideas</h1>
      </v-col>
    </v-row>
    <v-row >
        <NewIdea v-if="isAuthenticated" :select-categories="categories" @ideaCreated="getIdeas" />
    </v-row>
    <v-row align="center">
      <v-col v-for="(idea, index) in ideas.slice().reverse()" :key="index">
        <Idea :idea="idea" :all-categories="categories" />
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
    async getIdeas() {
      this.ideas = await this.$axios.$get('/ideas')
    },
  },
}
</script>
