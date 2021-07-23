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

    <v-row>
      <v-col>
        <h4>Members</h4>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-card v-if="project.team.length > 0" class="mx-auto">
          <v-list-item v-for="(member, index) in project.team" :key="index">
            <v-list-item-content>
              <v-list-item-title> {{ member }} </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-card>
        <v-card v-else class="mx-auto">
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title> No members </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <h4>Ideas</h4>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-card v-if="project.ideas.length > 0" class="mx-auto">
          <v-list-item v-for="(idea, index) in project.ideas" :key="index">
            <v-list-item-content>
              <v-list-item-title> {{ idea }} </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-card>
        <v-card v-else class="mx-auto">
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title> No ideas yet </v-list-item-title>
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
        <v-card v-if="project.comments.length > 0" class="mx-auto" tile>
          <v-list-item v-for="(comment, index) in project.comments.reverse()" :key="index">
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
  data: () => ({
    comment: null,
  }),
  computed: {
    ...mapGetters(['loggedInUser']),
  },
  methods: {
    async refresh() {
      this.project = await this.$axios.$get(`/projects/${this.id}`)
    },
    async postComment() {
      try {
        await this.$axios.post(`/projects/${this.id}/comments`, {
          comment: this.comment,
          author: this.loggedInUser.id
        })

        this.project = await this.$axios.$get(`/projects/${this.id}`)
      } catch (e) {
        this.error = e.response.data.message
      }

      this.comment = null
    }
  },
  async asyncData({ $axios, params }) {
    return {
      id: params.id,
      project: await $axios.$get(`/projects/${params.id}`),
      categories: await $axios.$get('/categories'),
    }
  },
}
</script>
