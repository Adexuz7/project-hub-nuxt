<template>
  <v-container>
    <v-col>
      <v-row>
        <teamCreation v-if="isAuthenticated"></teamCreation>
      </v-row>
    </v-col>
    <v-col>
      <v-row>
        <v-col v-for="(team,index) in myTeams" :key="index">
          <team :team="team"></team>
        </v-col>
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
  async asyncData({ $axios }) {
    return {
      myTeams: await $axios.$get('/teams'),
      // myTeams: await $axios.$get('/users/myteams'),
    }
  },
  created() {
    this.$root.$on('team-created', (newTeam) => {
      this.myTeams.push(newTeam)
    })

    this.$root.$on('member-added', async () => {
      this.myTeams = await this.$axios.$get('/teams')
    })
  }
}
</script>