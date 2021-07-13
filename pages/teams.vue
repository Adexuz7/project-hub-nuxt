<template>
  <v-container>
    <v-row class="teams-text">
      <v-col></v-col>
      <v-col></v-col>
      <v-col></v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    async asyncData({ $axios }) {
      return {
        ideas :await $$axios.$post('/teams',{
          
        })
      }
    } 
  }
</script>

<style  scoped>

</style>