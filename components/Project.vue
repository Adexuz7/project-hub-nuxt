<template>
  <v-card class="border-project mx-auto" width="360" height="500" outlined>
    <v-img
      gradient="to top right, rgba(100,115,201,.33), rgba(25,32,72,.7)"
      class="align-center justify-center"
      height="150px"
      :src="project.image"
    >
      <v-list-item two-line>
        <v-list-item-content>
          <v-list-item-title class="white--text text-h5">
            <span>
              <b>{{ project.name }}</b>
            </span>
          </v-list-item-title>
          <v-list-item-subtitle class="white--text">
            <span>
              <b>{{ date }}</b>
            </span>
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-img>

    <v-card-text>
      <v-row align="center">
        <v-col>
          <span class="description">
            <b>{{ description }}</b>
          </span>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-chip
            class="border-label px-5"
            :input-value="false"
            label
            outlined
            @click="toGitHub"
          >
            <v-icon small left> mdi-github </v-icon>
            <span class="ml-1"> GitHub </span>
          </v-chip>
          <v-chip class="border-label px-5" :input-value="false" label outlined>
            <v-icon small left> mdi-folder </v-icon>
            <span class="ml-1"> {{ categories }} </span>
          </v-chip>
        </v-col>
      </v-row>

      <v-row class="my-0">
        <v-col>
          <v-chip
            v-if="liked"
            class="border-label px-5"
            color="#FF6D00"
            label
            outlined
            @click="like"
          >
            <v-icon small left> mdi-thumb-up </v-icon>
            <span class="ml-1"> {{ likes }} </span>
          </v-chip>
          <v-chip v-else class="border-label px-5" label outlined @click="like">
            <v-icon small left> mdi-thumb-up </v-icon>
            <span class="ml-1"> {{ likes }} </span>
          </v-chip>
          <v-chip class="border-label px-5" :input-value="false" label outlined>
            <v-icon small left> mdi-comment </v-icon>
            <span class="ml-1"> {{ comments }} </span>
          </v-chip>
          <v-chip class="border-label px-5" label outlined>
            <v-icon small left> mdi-account-group </v-icon>
            <span class="ml-1"> {{ team }} </span>
          </v-chip>
          <v-chip class="border-label px-5" :input-value="false" label outlined>
            <v-icon small left> mdi-lightbulb </v-icon>
            <span class="ml-1"> {{ ideas }} </span>
          </v-chip>
        </v-col>
      </v-row>
    </v-card-text>

    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn class="mr-1" color="#FF6D00" text @click="seeMoreDetails"
        >More details</v-btn
      >
    </v-card-actions>
  </v-card>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  props: {
    project: {
      type: Object,
      default: null,
    },
  },
  computed: {
    liked() {
      if (this.isAuthenticated)
        return this.project.likes.includes(this.loggedInUser._id)
      return false
    },
    date() {
      return new Date(this.project.date).toDateString()
    },
    description() {
      if (this.project.description.length >= 300) {
        return this.project.description.substring(0, 300) + ' ...'
      } else {
        return this.project.description
      }
    },
    categories() {
      return this.project.categories.join(', ')
    },
    likes() {
      return this.project.likes.length
    },
    team() {
      return this.project.team.length
    },
    comments() {
      return this.project.comments.length
    },
    ideas() {
      return this.project.ideas.length
    },
    ...mapGetters(['isAuthenticated', 'loggedInUser']),
  },
  methods: {
    async like() {
      if (this.isAuthenticated) {
        const newProject = await this.$axios.put(
          '/projects/likes/' + this.project._id
        )

        this.$emit('addlike', newProject.data)
      } else {
        this.$router.push({ path: '/login', query: { requiresAuth: true } })
      }
    },
    toGitHub() {
      window.open(this.project.repository)
    },
    seeMoreDetails() {
      this.$router.push(`/projects/${this.project._id}`)
    },
  },
}
</script>

<style scoped>
.border-project {
  border-color: #ffab00;
}

.border-label {
  border: none;
}

.description {
  display: block;
  height: 154px;
  text-align: justify;
}

/* .container {
  background-image: url("../assets/50-Beautiful-and-Minimalist-Presentation-Backgrounds-02.jpg");
  background-size: cover;
  background-repeat: no-repeat;
} */
</style>