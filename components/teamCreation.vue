<template>
  <v-form v-model="valid">
    <v-container>
      <v-row>
        <v-col>
          <v-text-field
            v-model="teamName"
            label="Team name"
            :rules="nameRules"
            required
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-text-field v-model="description" label="team description" counter> </v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
      @click="submit"
    >
      Validate
    </v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  data:() =>({
    valid: false,
    teamName: '',
    nameRules:[
      v => !!v || 'Name is required',
    ],
    description:''
  }),
  methods:{
    async submit() {
      await this.$axios.post('/teams', {name: this.teamName, description: this.description, members:[],projects:[], messages:[]})
  }
}
}
</script>