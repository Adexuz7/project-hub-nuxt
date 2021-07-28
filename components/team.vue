<template>
  <div>
    <v-card
      class="border-project mx-auto my-5"
      width="360"
      height="360"
      outlined
    >
      <v-img
        gradient="to top right, rgba(100,115,201,.33), rgba(25,32,72,.7)"
        class="align-center justify-center"
        height="150px"
        :src="image"
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
            <span class="description">{{team.description}} </span>
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
    <v-overlay :value="overlay" :dark="false">
      <v-form>
        <v-card>
          <v-card-text>
            <v-row class="mb-3">
              <v-col>
                <v-btn color="error" outlined block @click="closeModal"
                  >X</v-btn
                >
              </v-col>
            </v-row>
            <v-text-field
              label="Member username"
              v-model="username"
              @keyup="getUsers"
              outlined
              dense
            ></v-text-field>

            <v-list v-if="this.users.length > 0">
              <v-list-item
                v-for="(user, idx) in users"
                :key="idx"
                @click="selectUser"
                >{{ user.userName }}</v-list-item
              >
            </v-list>
            <v-btn
              color="primary"
              outlined
              block
              @click="addNewMember(team._id)"
              >Add Member</v-btn
            >
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
      username: '',
      users: [],
    }
  },
  props: {
    team: {
      type: Object,
      default: null,
    },
  },
  computed: {
    image() {
      return (
        this.team.image ||
        'https://media.istockphoto.com/photos/software-developing-team-working-in-the-office-picture-id996082438?k=6&m=996082438&s=612x612&w=0&h=NgeMJY2DdiaC-o2ZQhxCsFy9XRG9DlMf2PwL5_PhHnw='
      )
    },
  },
  methods: {
    openModal() {
      this.overlay = true
    },
    closeModal() {
      this.overlay = false
    },
    async getUsers() {
      this.users = await this.$axios.$get(`/users/query?input=${this.username}`)
      console.log('holowos', this.users)
    },
    async addNewMember(teamId) {
      await this.$axios.$put(`/teams/${teamId}/users/${this.username}`)
      this.overlay = false
      this.$root.$emit('member-added')
    },
    selectUser(e) {
      this.username = e.target.innerText
    },
  },
}
</script>
