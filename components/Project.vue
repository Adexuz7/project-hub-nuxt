<template>
  <v-card class="mx-auto border-project" :width="width" :height="height" outlined>
    <v-img
      gradient="to top right, rgba(100,115,201,.33), rgba(25,32,72,.7)"
      class="align-center justify-center"
      height="150px"
      :src="image"
    >
      <v-list-item two-line>
        <v-list-item-content>
          <v-list-item-title class="text-h5 white--text">
            <span>
              <b>{{ project.name }}</b>
            </span>
          </v-list-item-title>
          <v-list-item-subtitle class="text-subtitle-2 white--text">
            <span>
              <b>{{ date }}</b>
            </span>
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-img>

    <v-card-text class="text-body-2">
      <v-row>
        <v-col>
          <span v-if="postIt" class="description">{{ description }}</span>
          <span v-else>{{ description }}</span>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-chip
            v-if="project.repository"
            class="border-label"
            :input-value="false"
            outlined
            @click="toGitHub"
          >
            <v-icon left> mdi-github </v-icon>
            <span> GitHub </span>
          </v-chip>
          <v-chip class="border-label" :input-value="false" outlined>
            <v-icon left> mdi-folder </v-icon>
            <span> {{ categories }} </span>
          </v-chip>
        </v-col>
      </v-row>

      <v-row class="my-0">
        <v-col>
          <v-chip
            v-if="liked"
            class="border-label"
            color="#FF6D00"
            outlined
            @click="like"
          >
            <v-icon left> mdi-thumb-up </v-icon>
            <span> {{ likes }} </span>
          </v-chip>
          <v-chip v-else class="border-label" outlined @click="like">
            <v-icon left> mdi-thumb-up </v-icon>
            <span> {{ likes }} </span>
          </v-chip>
          <v-chip class="border-label" :input-value="false" outlined>
            <v-icon left> mdi-comment </v-icon>
            <span> {{ comments }} </span>
          </v-chip>
          <v-chip class="border-label" outlined>
            <v-icon left> mdi-account-group </v-icon>
            <span> {{ team }} </span>
          </v-chip>
          <v-chip class="border-label" :input-value="false" outlined>
            <v-icon left> mdi-lightbulb </v-icon>
            <span> {{ ideas }} </span>
          </v-chip>
        </v-col>
      </v-row>
    </v-card-text>

    <v-card-actions v-if="postIt">
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
  name: 'Project',
  props: {
    project: {
      type: Object,
      default: null,
    },
    postIt: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    width() {
      if (this.postIt) return 360
      return '100%'
    },
    height() {
      if (this.postIt) return 500
      return '100%'
    },
    image() {
      return (
        this.project.image ||
        'https://images.unsplash.com/photo-1531403009284-440f080d1e12?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib'
      )
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
      return this.project.categories.map((e) => e.name).join(', ')
    },
    liked() {
      if (this.isAuthenticated)
        return this.project.likes.includes(this.loggedInUser._id)
      return false
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