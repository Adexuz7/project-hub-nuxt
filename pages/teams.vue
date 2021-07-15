<template>
 <div>
   <teamCreation v-if="isAuthenticated"></teamCreation>
 </div>
</template>

<script>
import { mapGetters } from 'vuex'
import teamCreation from '~/components/teamCreation.vue'
  export default {
  components: { teamCreation },
   computed: {
    ...mapGetters(['isAuthenticated']),
  },

  }
  
</script>