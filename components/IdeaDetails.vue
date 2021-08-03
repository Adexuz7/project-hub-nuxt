<template>
  <v-card class="border-idea mx-auto" outlined>
    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title class="text-h5">
          <span>{{ idea.name }}</span>
        </v-list-item-title>
        <v-list-item-subtitle>
          <span>{{ date }}, {{ idea.author.name }}</span>
        </v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-card-text>
      <v-row>
        <v-col>
          <span>{{ idea.description }}</span>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-chip class="border-label px-5" :input-value="false" label outlined>
            <v-icon small left> mdi-folder </v-icon>
            <span class="ml-1"> {{ categories }} </span>
          </v-chip>
          <v-chip class="border-label px-5" :input-value="false" label outlined>
            <v-icon small left> mdi-arm-flex </v-icon>
            <span class="ml-1"> {{ idea.difficulty }} </span>
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
          <v-chip class="border-label px-5" :input-value="false" label outlined>
            <v-icon small left> mdi-file </v-icon>
            <span class="ml-1"> {{ projects }} </span>
          </v-chip>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  props: {
    idea: {
      type: Object,
      default: null,
    },
  },
  computed: {
    liked() {
      if (this.isAuthenticated)
        return this.idea.likes.includes(this.loggedInUser._id)
      return false
    },
    date() {
      return new Date(this.idea.date).toDateString()
    },
    categories() {
      const categories = []

      this.idea.categories.forEach((category) => {
          categories.push(category.name)
      })

      return categories.join(', ')
    },
    likes() {
      return this.idea.likes.length
    },
    comments() {
      return this.idea.comments.length
    },
    projects() {
      return this.idea.projects.length
    },
    ...mapGetters(['isAuthenticated', 'loggedInUser']),
  },
  methods: {
    async like() {
      if (this.isAuthenticated) {
        const idea = await this.$axios.put(`/ideas/likes/${this.idea._id}`)
        this.$emit('like', idea.data)
      } else {
        this.$router.push({ path: '/login', query: { requiresAuth: true } })
      }
    },
  },
}
</script>

<style scoped>
.border-idea {
  border-color: #ffab00;
}

.border-label {
  border: none;
}

.description {
  display: block;
  height: 90px;
  text-align: justify;
}
</style>
