<template>
  <v-container>
    <v-row class="text-center">
      <v-col>
        <h1>Explore ideas</h1>
      </v-col>
    </v-row>
    <v-row class="text-center">
      <v-col>
        <v-btn @click="getIdeas">
          <v-icon>mdi-refresh</v-icon>
        </v-btn>
      </v-col>
    </v-row>
    <v-row v-if="isAuthenticated">
      <v-col>
        <NewIdea :select-categories="categories" @ideaCreated="getIdeas" />
      </v-col>
    </v-row>
    <v-row>
      <v-col v-for="(idea, index) in ideas" :key="index">
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
    ...mapGetters(['isAuthenticated', 'loggedInUser']),
  },
  // data: () => ({
  //   token: this.$auth.$storage.getLocalStorage('token'),
  // }),
  methods: {
    async getIdeas() {
      this.ideas = await this.$axios.$get('/ideas')
    },
  },
}

// export default {
//   data: () => ({
//     token: localStorage.token,
//   }),
</script>
