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
        @ideaCreated="getIdeas"
      />
    </v-row>
    <v-row align="center">
      <v-col v-for="(idea, index) in ideas.slice().reverse()" :key="index">
        <Idea
          :idea="idea"
          :all-categories="categories"
          @addLikesIdea="addLikeIdea"
        />
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
    addLikeIdea(idea) {
      const arr = this.ideas.map((idea) => idea._id)

      this.ideas.splice(arr.indexOf(idea._id), 1, idea)
    },
  },
}
</script>

<style scoped>
h1 {
  display: flex;
  justify-content: center;
  font-family: 'Satisfy', 'Montserrat Alternates', sans-serif;
  letter-spacing: 10px;
}
</style>
