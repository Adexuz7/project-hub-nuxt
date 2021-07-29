<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Project</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <ProjectDetails
          :project="project"
          :all-categories="categories"
          @like="refresh"
        />
      </v-col>
    </v-row>

    <v-row v-if="project.team.length > 0">
      <v-col>
        <v-card outlined>
          <v-card-title>Teams</v-card-title>
          <v-card-text>
            <v-row>
              <v-col>
                <v-card v-if="project.team.length > 0" class="mx-auto" outlined>
                  <v-list-item
                    v-for="(team, index) in project.team"
                    :key="index"
                  >
                    <v-list-item-content>
                      <v-list-item-title> {{ team.name }} </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-card>
                <v-card v-else class="mx-auto" outlined>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title> No teams </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <v-row v-if="project.ideas.length > 0">
      <v-col>
        <v-card outlined>
          <v-card-title>Ideas</v-card-title>
          <v-card-text>
            <v-row v-if="project.ideas.length > 0">
              <v-col>
                <v-card class="mx-auto" outlined>
                  <div v-for="(idea, index) in project.ideas" :key="index">
                    <v-list-item :to="`/ideas/${idea._id}`">
                      <v-list-item-content>
                        <v-list-item-title> {{ idea.name }} </v-list-item-title>
                      </v-list-item-content>
                    </v-list-item>
                  </div>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-card outlined>
          <v-card-title>Comments</v-card-title>
          <v-card-text>
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
                <v-card
                  v-if="project.comments.length > 0"
                  class="mx-auto"
                  outlined
                >
                  <v-list-item
                    v-for="(comment, index) in comments"
                    :key="index"
                  >
                    <Comment :comment="comment" />
                  </v-list-item>
                </v-card>
                <v-card v-else class="mx-auto" outlined>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title> No comments </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
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
      project: await $axios.$get(`/projects/${params.id}`),
      categories: await $axios.$get('/categories'),
    }
  },
  data: () => ({
    comment: null,
  }),
  computed: {
    ...mapGetters(['loggedInUser']),
    comments() {
      return this.project.comments.slice().reverse()
    },
  },
  methods: {
    async refresh() {
      this.project = await this.$axios.$get(`/projects/${this.id}`)
    },
    async postComment() {
      try {
        await this.$axios.post(`/projects/${this.id}/comments`, {
          comment: this.comment,
          author: this.loggedInUser.id,
        })

        this.project = await this.$axios.$get(`/projects/${this.id}`)
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
