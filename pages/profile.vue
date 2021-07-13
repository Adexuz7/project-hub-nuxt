<template>
  <v-container v-if="token">
    <v-row justify="space-around">
      <v-col>
        <v-card class="mx-auto" max-width="600">
          <v-img
            class="white--text align-end"
            height="200px"
            src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
          >
            <v-card-title>
              <v-avatar size="56">
                <img
                  alt="user"
                  src="https://cdn.pixabay.com/photo/2020/06/24/19/12/cabbage-5337431_1280.jpg"
                />
              </v-avatar>
              <span class="ml-3">{{ user.name }}</span>
            </v-card-title>
          </v-img>

          <v-card-text class="text--primary">
            <v-row class="text-center">
              <v-col>
                <span>{{ user.email }}</span>
              </v-col>
            </v-row>

            <v-row class="text-center">
              <v-col>
                <v-icon>mdi-account-star</v-icon>
                <span class="ml-2">{{ user.follows.length }}</span>
              </v-col>
              <v-col>
                <v-icon>mdi-account-multiple-check</v-icon>
                <span class="ml-2">{{ user.followers.length }}</span>
              </v-col>
            </v-row>

            <v-row>
              <v-expansion-panels flat>
                <v-expansion-panel>
                  <v-expansion-panel-header>Ideas</v-expansion-panel-header>
                  <v-expansion-panel-content
                    v-for="(idea, index) in user.ideas"
                    :key="index"
                  >
                    <v-list-item dense>
                      <v-list-item-content>
                        <v-list-item-title>{{ idea }}</v-list-item-title>
                      </v-list-item-content>
                    </v-list-item>
                  </v-expansion-panel-content>
                </v-expansion-panel>
              </v-expansion-panels>
            </v-row>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn text color="deep-purple accent-4" @click="logOut">
              <span>Log out</span>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <!-- <v-row>
      <v-col>
        <p>{{ user }}</p>
      </v-col>
    </v-row> -->
  </v-container>
  <Login v-else @login="updateToken" />
</template>

<script>
export default {
  data: () => ({
    // token: localStorage.token,
    token: false,
    user: null,
  }),
  methods: {
    updateToken() {
      // this.token = localStorage.token
      // console.log('Update the token')
    },
    logOut() {
      // localStorage.removeItem("token")
      // localStorage.removeItem("id")
      // localStorage.removeItem("name")
      // localStorage.removeItem("email")
      this.user = null
    },
    // async mounted() {
    //   this.user = await usersService.getUser(localStorage.id)
    //   // this.allIdeas = await ideasService.getAllIdeas();
    // },
  },
}
</script>
