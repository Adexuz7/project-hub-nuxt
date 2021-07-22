<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Idea</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <IdeaDetails
          :idea="idea"
          :all-categories="categories"
          @addLikesIdea="refreshIdea"
        />
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <h4>Projects</h4>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-card v-if="idea.projects.length > 0" class="mx-auto">
          <v-list-item v-for="(project, index) in idea.projects" :key="index">
            <v-list-item-content>
              <v-list-item-title> {{ project }} </v-list-item-title>
            </v-list-item-content>
            <!-- <Comment :comment="comment" /> -->
          </v-list-item>
        </v-card>
        <v-card v-else class="mx-auto">
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title> No projects yet </v-list-item-title>
            </v-list-item-content>
            <!-- <Comment :comment="comment" /> -->
          </v-list-item>
        </v-card>

        <!-- <div v-for="(comment, index) in idea.comments" :key="index"> -->
        <!-- <p>{{comment}}</p> -->
        <!-- </div> -->
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <h4>Comments</h4>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-text-field
          v-if="loggedUser"
          v-model="comment"
          label="Add a comment"
          append-icon="mdi-send"
          @click:append="postComment"
          @keyup.enter="postComment"
          outlined
          dense
        ></v-text-field>
        <v-text-field
          v-else
          label="Log in to be able to comment"
          append-icon="mdi-send"
          disabled
          outlined
          dense
        ></v-text-field>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-card v-if="idea.comments.length > 0" class="mx-auto" tile>
          <v-list-item v-for="(comment, index) in idea.comments" :key="index">
            <!-- <v-list-item-content>
              <v-list-item-title>Single-line item</v-list-item-title>
            </v-list-item-content> -->
            <Comment :comment="comment" />
          </v-list-item>
        </v-card>
        <v-card v-else class="mx-auto" tile>
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title> No comments yet </v-list-item-title>
            </v-list-item-content>
            <!-- <Comment :comment="comment" /> -->
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
import { mapGetters } from 'vuex'

export default {
  auth: false,
  data: () => ({
    comment: null,
  }),
  computed: {
    ...mapGetters(['loggedInUser']),
  },
  async asyncData({ $axios, params }) {
    return {
      id: params.id,
      idea: await $axios.$get(`/ideas/${params.id}`),
      categories: await $axios.$get('/categories'),
    }
  },
  methods: {
    async refreshIdea() {
      this.idea = await this.$axios.$get(`/ideas/${this.id}`)
    },
    async postComment() {
      try {
        await this.$axios.post(`/ideas/${this.id}/comments`, {
          comment: this.comment,
        })

        this.idea = await this.$axios.$get(`/ideas/${this.id}`)
      } catch (e) {
        this.error = e.response.data.message
      }

      this.comment = null
    },
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
