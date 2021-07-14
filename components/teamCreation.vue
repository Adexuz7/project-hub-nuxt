<template>
  <v-form v-model="valid">
    <v-container>
      <v-row>
        <v-col>
            <v-text-field v-model="teamName" label="Team name" :rules="nameRules" required></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-text-field v-model="description">
          </v-text-field>
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

  // methods:{
  //   submit:submit()
  // }
}
</script>