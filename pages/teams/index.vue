<template>
  <v-container>
    <v-row>
      <v-col>
        <teamCreation v-if="isAuthenticated"></teamCreation>
      </v-col>
    </v-row>
    <v-row v-for="(team, index) in teams" :key="index">
      <v-col>
        <Team :team="team"/>
      </v-col>
    </v-row>
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
    const teams = await $axios.$get('/teams')
    return { teams }
  },
}
</script>
