<template>
  <v-container>
    <v-row justify="space-around">
      <v-col>
        <v-card class="mx-auto" max-width="600" outlined>
          <v-img
            class="white--text align-end"
            height="200px"
            :src="banner"
          >
            <v-card-title>
              <v-avatar size="56">
                <img
                  alt="user"
                  :src="image"
                />
              </v-avatar>
              <span class="ml-3">{{ loggedInUser.name }}</span>
            </v-card-title>
          </v-img>

          <v-card-text class="text--primary">
            <v-row class="text-center">
              <v-col>
                <span>{{ loggedInUser.email }}</span>
              </v-col>
            </v-row>

            <!-- <v-row class="text-center">
              <v-col>
                <v-icon>mdi-account-star</v-icon>
                <span class="ml-2">{{ loggedInUser.follows.length }}</span>
              </v-col>
              <v-col>
                <v-icon>mdi-account-multiple-check</v-icon>
                <span class="ml-2">{{ loggedInUser.followers.length }}</span>
              </v-col>
            </v-row> -->

            <v-row>
              <v-expansion-panels flat>
                <v-expansion-panel>
                  <v-expansion-panel-header>Ideas</v-expansion-panel-header>
                  <v-expansion-panel-content
                    v-for="(idea, index) in loggedInUser.ideas"
                    :key="index"
                  >
                    <v-list-item :to="`/ideas/${ idea._id }`" dense>
                      <v-list-item-content>
                        <v-list-item-title>{{ idea.name }}</v-list-item-title>
                      </v-list-item-content>
                    </v-list-item>
                  </v-expansion-panel-content>
                </v-expansion-panel>
              </v-expansion-panels>
            </v-row>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn text color="#FF6D00" @click="logOut">
              <span>Log out</span>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  middleware: 'auth',
  computed: {
    ...mapGetters(['loggedInUser']),
    image() {
      return this.loggedInUser.image || 'https://cdn.pixabay.com/photo/2020/06/24/19/12/cabbage-5337431_1280.jpg'
    },
    banner() {
      return this.loggedInUser.banner || 'https://cdn.vuetifyjs.com/images/cards/docks.jpg'
    }
  },
  methods: {
    async logOut() {
      await this.$auth.logout()
    },
  },
}
</script>
