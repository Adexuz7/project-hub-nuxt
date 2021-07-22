<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Idea</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <IdeaDetails :idea="idea" :all-categories="categories" @addLikesIdea="addLikeIdea" />
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-card class="mx-auto" tile>
          <v-list-item v-for="(comment, index) in idea.comments" :key="index">
            <!-- <v-list-item-content>
              <v-list-item-title>Single-line item</v-list-item-title>
            </v-list-item-content> -->
            <Comment :comment="comment" />
          </v-list-item>
        </v-card>

        <!-- <div v-for="(comment, index) in idea.comments" :key="index"> -->
        <!-- <p>{{comment}}</p> -->
        <!-- </div> -->
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  auth: false,
  async asyncData({ $axios, params }) {
    return {
      id: params.id,
      idea: await $axios.$get(`/ideas/${ params.id }`),
      categories: await $axios.$get('/categories'),
    }
  },
  methods: {
    async addLikeIdea() {
      this.idea = await this.$axios.$get(`/ideas/${ this.id }`)
    }
  },
}

// import { mapGetters } from 'vuex'

// export default {
//   auth: false,
//   async asyncData({ $axios }) {
//     return {
//       ideas: await $axios.$get('/ideas'),
//       categories: await $axios.$get('/categories'),
//     }
//   },
//   computed: {
//     ...mapGetters(['isAuthenticated']),
//   },
//   methods: {
//     async getIdeas() {
//       this.ideas = await this.$axios.$get('/ideas')
//     },
//   },
// }
</script>
