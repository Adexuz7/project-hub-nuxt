<template>
  <div>
    <v-card class="border-project mx-auto my-5" width="360" outlined>
      <v-img
        gradient="to top right, rgba(100,115,201,.33), rgba(25,32,72,.7)"
        class="align-center justify-center"
        height="150px"
      >
        <v-list-item two-line>
          <v-list-item-content>
            <v-list-item-title class="white--text text-h5">
              <span>
                <b>{{ team.name }}</b>
              </span>
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-img>

      <v-card-text>
        <v-row align="center">
          <v-col>
            <span class="description">
              <b>{{ team.description }}</b>
            </span>
          </v-col>
        </v-row>

        <v-row>
          <v-col>
            <v-chip
              v-for="(member, idx) in team.members"
              :key="idx"
              class="border-label px-5 mx-1"
              :input-value="false"
              label
              outlined
            >
              <v-icon small left> mdi-account </v-icon>
              <span class="ml-1"> {{ member.name }} </span>
            </v-chip>
          </v-col>
        </v-row>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn class="mr-1" color="#FF6D00" text @click="openModal()"
          >Add Member</v-btn
        >
      </v-card-actions>
    </v-card>
    <v-overlay :value="overlay">
      <v-form>
        <v-card>
          <v-card-text>
            <v-text-field label="Member username" v-model="username">
            </v-text-field>
            <v-btn @click="addNewMember(team._id)">Add Member</v-btn>
          </v-card-text>
        </v-card>
      </v-form>
    </v-overlay>
  </div>
</template>

<script>
export default {
  data() {
    return {
      overlay: false,
      username: ''
    }
  },
  props: {
    team: {
      type: Object,
      default: null,
    },
  },
  methods: {
    openModal() {
      this.overlay = true
    },
    async addNewMember(teamId) {
      await this.$axios.$put(`/teams/${teamId}/users/${this.username}`)
      this.overlay = false
      this.$root.$emit('member-added')
    },
  },
}
</script>
