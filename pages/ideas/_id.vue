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
          @like="refresh"
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
        <v-card v-if="idea.projects.length > 0" class="mx-auto" outlined>
          <div v-for="(project, index) in idea.projects" :key="index">
            <v-list-item :to="`/projects/${project._id}`">
              <v-list-item-content>
                <v-list-item-title> {{ project.name }} </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </div>
        </v-card>
        <v-card v-else class="mx-auto" outlined>
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title> No projects yet </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <h4>Comments</h4>
      </v-col>
    </v-row>

    <v-row>
      <v-col v-if="loggedInUser">
        <v-text-field
          v-model="comment"
          label="Add a comment"
          append-icon="mdi-send"
          outlined
          dense
          @click:append="postComment"
          @keyup.enter="postComment"
        ></v-text-field>
      </v-col>
      <v-col v-else>
        <v-text-field
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
        <v-card v-if="idea.comments.length > 0" class="mx-auto" outlined>
          <v-list-item v-for="(comment, index) in comments" :key="index">
            <Comment :comment="comment" />
          </v-list-item>
        </v-card>
        <v-card v-else class="mx-auto" outlined>
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title> No comments yet </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  auth: false,
  async asyncData({ $axios, params }) {
    return {
      id: params.id,
      idea: await $axios.$get(`/ideas/${params.id}`),
      categories: await $axios.$get('/categories'),
    }
  },
  data: () => ({
    comment: null,
  }),
  computed: {
    ...mapGetters(['loggedInUser']),
    comments() {
      return this.idea.comments.slice().reverse()
    },
  },
  methods: {
    async refresh() {
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
</script>

<style>
h1 {
  font-family: 'Montserrat', sans-serif;
}

h4 {
  font-family: 'Montserrat', sans-serif;
}
</style>