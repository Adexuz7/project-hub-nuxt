<template>
  <v-card class="border-idea mx-auto" :width="width" :height="height" outlined>
    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title class="text-h6">
          <span>{{ idea.name }}</span>
        </v-list-item-title>
        <v-list-item-subtitle class="text-subtitle-2">
          <span>{{ date }}, {{ idea.author.name }}</span>
        </v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-card-text class="text-body-2">
      <v-row>
        <v-col>
          <span v-if="postIt" class="description">{{ description }}</span>
          <span v-else>{{ description }}</span>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-chip class="border-label" :input-value="false" outlined>
            <v-icon left> mdi-folder </v-icon>
            <span> {{ categories }} </span>
          </v-chip>
          <v-chip class="border-label" :input-value="false" outlined>
            <v-icon left> mdi-arm-flex </v-icon>
            <span> {{ idea.difficulty }} </span>
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
          <v-chip class="border-label" :input-value="false" outlined>
            <v-icon left> mdi-file </v-icon>
            <span> {{ projects }} </span>
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
  props: {
    idea: {
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
      if (this.postIt) return 358
      return '100%'
    },
    liked() {
      if (this.isAuthenticated)
        return this.idea.likes.includes(this.loggedInUser._id)
      return false
    },
    date() {
      return new Date(this.idea.date).toDateString()
    },
    description() {
      if (this.postIt && this.idea.description.length >= 199)
        return this.idea.description.substring(0, 199) + ' ...'
      return this.idea.description
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
        const newIdea = await this.$axios.put('/ideas/likes/' + this.idea._id)

        this.$emit('like', newIdea.data)
      } else {
        this.$router.push({ path: '/login', query: { requiresAuth: true } })
      }
    },
    seeMoreDetails() {
      this.$router.push(`/ideas/${this.idea._id}`)
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
