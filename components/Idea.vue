<template>
  <v-card
    class="mx-auto"
    min-width="296"
    max-width="400"
    color="yellow lighten-2"
  >
    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title class="text-h5">
          <span>{{ idea.name }}</span>
        </v-list-item-title>
        <v-list-item-subtitle>
          <span>{{ date }}, {{ author.name }}</span>
        </v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-card-text>
      <v-row align="center">
        <v-col>
          <span>{{ description }}</span>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-folder</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>
              <span>{{ categories }}</span>
            </v-list-item-subtitle>
          </v-list-item>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-arm-flex</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>
              <span>{{ idea.difficulty }}</span>
            </v-list-item-subtitle>
          </v-list-item>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-thumb-up</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>
              <span>{{ likes }}</span>
            </v-list-item-subtitle>
          </v-list-item>
        </v-col>
        <v-col>
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-comment</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>
              <span>{{ comments }}</span>
            </v-list-item-subtitle>
          </v-list-item>
        </v-col>

        <v-col>
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-file</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>
              <span>{{ projects }}</span>
            </v-list-item-subtitle>
          </v-list-item>
        </v-col>
      </v-row>
    </v-card-text>

    <v-divider></v-divider>

    <!-- <v-card-actions>
      <v-btn text>More details</v-btn>
    </v-card-actions> -->
  </v-card>
</template>

<script>
export default {
  props: {
    idea: {},
    allCategories: [],
  },
  data: () => ({
    author: 'Unknown',
  }),
  computed: {
    date() {
      return new Date(this.idea.date).toDateString()
    },
    description() {
      if (this.idea.description.length >= 140) {
        return this.idea.description.substring(0, 140) + ' ...'
      } else {
        return this.idea.description
      }
    },
    categories() {
      const categoriesNames = []

      this.allCategories.forEach((category) => {
        this.idea.categories.forEach((ideaCategory) => {
          if (ideaCategory === category._id) categoriesNames.push(category.name)
        })
      })

      return categoriesNames.join(', ')
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
  },
  async mounted() {
    this.author = await this.$axios.$get(`/users/${this.idea.author}`)
  },
}
</script>
