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
        <v-card outlined>
          <v-card-title>Projects</v-card-title>
          <v-card-text>
            <v-row>
              <v-col>
                <v-btn block outlined @click="openModal">Add project</v-btn>
                <v-overlay :dark="false" :value="overlay">
                  <v-form>
                    <v-card>
                      <v-card-text>
                        <v-btn
                          class="mb-4"
                          color="error"
                          block
                          outlined
                          @click="closeModal"
                          >x</v-btn
                        >
                        <v-text-field
                          v-model="name"
                          label="Name"
                          outlined
                          dense
                        ></v-text-field>
                        <v-textarea
                          v-model="description"
                          label="Description"
                          no-resize
                          outlined
                          dense
                        ></v-textarea>
                        <v-select
                          v-model="projectCategories"
                          :items="categories"
                          label="Category"
                          item-value="id"
                          item-text="name"
                          return-object
                          outlined
                          dense
                        ></v-select>
                        <v-btn block color="primary" @click="newProject">
                          New project
                        </v-btn>
                      </v-card-text>
                    </v-card>
                  </v-form>
                </v-overlay>
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-card
                  v-if="idea.projects.length > 0"
                  class="mx-auto"
                  outlined
                >
                  <div v-for="(project, index) in idea.projects" :key="index">
                    <v-list-item :to="`/projects/${project._id}`">
                      <v-list-item-content>
                        <v-list-item-title>
                          {{ project.name }}
                        </v-list-item-title>
                      </v-list-item-content>
                    </v-list-item>
                  </div>
                </v-card>
                <v-card v-else class="mx-auto" outlined>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title> No projects </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- <v-row>
      <v-col>

      </v-col>
    </v-row> -->

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
                  v-if="idea.comments.length > 0"
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
      idea: await $axios.$get(`/ideas/${params.id}`),
      categories: await $axios.$get('/categories'),
    }
  },
  data: () => ({
    comment: null,
    overlay: false,
    name: '',
    description: '',
    projectCategories: [],
  }),
  computed: {
    ...mapGetters(['loggedInUser']),
    comments() {
      return this.idea.comments.slice().reverse()
    },
  },
  methods: {
    openModal() {
      this.overlay = true
    },
    closeModal() {
      this.overlay = false
    },
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
    async newProject() {
      await this.$axios.$post(`/ideas/${this.id}/projects`, {
        name: this.name,
        description: this.description,
        categories: this.categories,
      })

      this.overlay = false
      this.idea = await this.$axios.$get(`/ideas/${this.id}`)
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