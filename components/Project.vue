<template>
  <v-card
    class="mx-auto"
    min-width="296"
    max-width="600"
    color="orange lighten-3"
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
            <b>{{ description }}</b>
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
              <!-- <a @click="addlikes"><v-icon>mdi-thumb-up</v-icon></a> -->
              <a><v-icon>mdi-thumb-up</v-icon></a>
            </v-list-item-icon>
            <v-list-item-subtitle>
              <span>{{ likes }}</span>
            </v-list-item-subtitle>
          </v-list-item>
        </v-col>
        <v-col class="d-flex justify-center align-center">
          <a :href="project.repository" target="blank">
            <v-icon>mdi-book</v-icon>
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
      <v-row>
        <v-col>
          <v-expansion-panels flat>
            <v-expansion-panel>
              <v-expansion-panel-header color="orange lighten-4">
                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>mdi-comment</v-icon>
                  </v-list-item-icon>
                  <v-list-item-subtitle>
                    <span>{{ project.comments.length }}</span>
                  </v-list-item-subtitle>
                </v-list-item>
              </v-expansion-panel-header>

              <v-expansion-panel-content>
                <div v-for="(comment, index) in project.comments" :key="index">
                  <Comment :comment="comment" />
                </div>
              </v-expansion-panel-content>
            </v-expansion-panel>
          </v-expansion-panels>
        </v-col>
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
  },
  computed: {
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
    likes() {
      return this.project.likes.length
    },
  },
  // methods: {
  //   async addlikes() {
  //     const likes = this.project.likes
  //     if (!likes.includes(localStorage.email)) {
  //       likes.push(localStorage.email)
  //       console.log('likes1: ', likes.length)
  //     } else {
  //       likes.splice(likes.indexOf(localStorage.email), 1)
  //       console.log('likes2: ', likes.length)
  //     }

  //     const newProject = await projectsService.updateProjectById(this.project)
  //     console.log(newProject)
  //     this.$emit('addlike')
  //   },
  // },
}
</script>

<style scoped>
a {
  text-decoration: none;
}
</style>