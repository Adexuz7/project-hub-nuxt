<template>
   <v-card class="border-team mx-auto" outlined>
     {{team}}
    <v-list-item two-line>
      <v-list-item-content class="title">
        <v-list-item-title class="text-h5">
          <span class="title-idea">{{ team.name }}</span>
        </v-list-item-title>

      </v-list-item-content>
    </v-list-item>

    <v-card-text>
      <v-row>
        <v-col>
          <span class="description">{{ team.description }}</span>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-chip v-for="(member,idx) in team.members" :key="idx" class="border-label px-5" :input-value="false" label outlined>
            <v-icon small left> mdi-account </v-icon>
            <span class="ml-1"> {{member}}</span>
          </v-chip>

        </v-col>
      </v-row>
    </v-card-text>

    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn class="mr-1" color="#FF6D00" text >Add member</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    team: {
      type: Object,
      default: null,
    },
  },
  methods: {
    getMemberName: async (id) =>{
      return await this.axios.get(`/teams/${this.team._id}`)


    }
  },
}
</script>

<style scoped>
.border-team {
  border-color: #ffab00;
}
</style>
