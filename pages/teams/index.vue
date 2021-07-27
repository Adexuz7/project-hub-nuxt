<template>
  <v-container>
    <v-col>
      <v-row>
        <teamCreation v-if="isAuthenticated"></teamCreation>
      </v-row>
    </v-col>
    <v-col v-for="(team,index) in myTeams" :key="index">
      <v-row>
        <team :team="team"></team>
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex'
import Team from '~/components/team.vue'
import teamCreation from '~/components/teamCreation.vue'
export default {
  components: { teamCreation, Team },
  computed: {
    ...mapGetters(['isAuthenticated']),
  },
  async asyncdata({ $axios }) {
    return {
      allTeams: await $axios.$get('/teams'),
      myTeams: await $axios.$get('/users/myteams'),
    }
  },
  methods: {
    async myTeams() {
      this.myTeams = await this.$axios.$get('/users/myteams')
    },
  },
}
</script>