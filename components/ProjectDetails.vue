<template>
  <v-card
    class="mx-auto"
    min-width="296"
    max-width="600"
    color="projects"
  >
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
          <span>
            <b>{{ project.description }}</b>
          </span>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-folder</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>
              <span>{{ project.categories.join(', ') }}</span>
            </v-list-item-subtitle>
          </v-list-item>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-account-group</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>
              <span>{{ project.team.join(', ') }}</span>
            </v-list-item-subtitle>
          </v-list-item>
        </v-col>
        <v-col>
          <v-list-item>
            <v-list-item-icon>
              <a @click="like"><v-icon>mdi-thumb-up</v-icon></a>
              <!-- <a><v-icon>mdi-thumb-up</v-icon></a> -->
            </v-list-item-icon>
            <v-list-item-subtitle>
              <span>{{ likes }}</span>
            </v-list-item-subtitle>
          </v-list-item>
        </v-col>
        <v-col class="d-flex justify-center align-center">
          <a :href="project.repository" target="blank">
            <v-icon>mdi-github</v-icon>
          </a>
        </v-col>
        <!-- <v-col>
          <v-list-item>
            <v-list-item-icon>
              <button><v-icon>mdi-alert-circle-outline</v-icon></button>
            </v-list-item-icon>
            <v-list-item-subtitle>
              <span>{{ project.issues.join(", ") }}</span>
            </v-list-item-subtitle>
          </v-list-item>
        </v-col> -->
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    project: {
      type: Object,
      default: null,
    },
    allCategories: {
      type: Array,
      default: null,
    },
  },
  data: () => ({
    author: 'Unknown',
  }),
  computed: {
    date() {
      return new Date(this.project.date).toDateString()
    },
    categories() {
      const categoriesNames = []

      this.allCategories.forEach((category) => {
        this.project.categories.forEach((projectCategory) => {
          if (projectCategory === category._id) categoriesNames.push(category.name)
        })
      })

      return categoriesNames.join(', ')
    },
    likes() {
      return this.project.likes.length
    },
    comments() {
      return this.project.comments.length
    },
    ideas() {
      return this.project.ideas.length
    },
  },
  methods: {
    async like() {
      const project = await this.$axios.put(`/likes/${this.project._id}`)
      this.$emit('like', project.data)
    },
  },
  // async mounted() {
  //   this.author = await this.$axios.$get(`/users/${this.project.author}`)
  // },
}
</script>
