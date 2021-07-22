<template>
  <div class="Comentario">
    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title class="text-h7">
          <v-avatar size="35" class="mr-5 mb-2">
            <img
              alt="user"
              :src="image"
            />
          </v-avatar>
          <span
            ><b>{{ author.name }}</b></span
          >
        </v-list-item-title>
        <v-list-item-subtitle>
          <span
            ><b>{{ date }}</b></span
          >
        </v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>
    <v-row class="pt-5">
      <v-col>
        <span>{{ description }}</span>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-list-item>
          <v-list-item-icon>
            <v-icon>mdi-thumb-up</v-icon>
          </v-list-item-icon>
          <v-list-item-subtitle>
            <span>{{ likes }}</span>
          </v-list-item-subtitle>
        </v-list-item>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  props: {
    comment: {
      type: Object,
      default: null
    },
  },
  data: () => ({
    author: '',
  }),
  computed: {
    image() {
      return this.author.image || 'https://cdn.pixabay.com/photo/2020/06/24/19/12/cabbage-5337431_1280.jpg'
    },
    date() {
      return new Date(this.comment.date).toDateString()
    },
    description() {
      if (this.comment.comment.length >= 300) {
        return this.comment.comment.substring(0, 300) + ' ...'
      } else {
        return this.comment.comment
      }
    },
    likes() {
      return this.comment.likes.length
    },
  },
  async mounted() {
    this.author = await this.$axios.$get(`/users/${this.comment.author}`)
    this.allCategories = await this.$axios.$get('/categories')
  },
}
</script>

<style lang="scss" scoped>
.Comentario {
  padding: 10px;
}
</style>