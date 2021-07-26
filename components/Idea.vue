<template>
  <v-card class="border-idea mx-auto" width="360" height="358" outlined>
    <v-list-item two-line>
      <v-list-item-content class="title">
        <v-list-item-title class="text-h5">
          <span class="title-idea">{{ idea.name }}</span>
        </v-list-item-title>
        <v-list-item-subtitle>
          <span>{{ date }}, {{ author.name }}</span>
        </v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-card-text>
      <v-row>
        <v-col>
          <span class="description">{{ description }}</span>
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
          <v-chip class="border-label px-5" label outlined  @click="addLikesIdea">
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

    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn class="mr-1" color="#FF6D00" text @click="seeMoreDetails">More details</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    idea: {
      type: Object,
      default: null,
    },
    allCategories: {
      type: Array,
      default: null,
    },
  },
  data: () => ({
    author: {
      name: 'unknown',
    },
  }),
  computed: {
    date() {
      return new Date(this.idea.date).toDateString()
    },
    description() {
      if (this.idea.description.length >= 199) {
        return this.idea.description.substring(0, 199) + ' ...'
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
  methods: {
    async addLikesIdea() {
      const newIdea = await this.$axios.put('/ideas/likes/' + this.idea._id)

      this.$emit('addLikesIdea', newIdea.data)
    },
    seeMoreDetails() {
      this.$router.push(`/ideas/${this.idea._id}`)
    },
  },
}
</script>

<style scoped>
.border-idea {
  border-color: #FFAB00;
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
