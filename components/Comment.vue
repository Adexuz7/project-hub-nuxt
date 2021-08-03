no<template>
    <div class="comment">
      <v-list-item-content>
        <v-list-item-title>
          <v-avatar size="36" class="mr-2">
            <img alt="user" :src="image" />
          </v-avatar>
          <span>
            <a class="author" :href="`/users/${comment.author._id}`"><b>{{ comment.author.name }}</b></a> · {{ date }}
          </span>
        </v-list-item-title>
        <span class="ml-12">{{ description }}</span>
      </v-list-item-content>
    </div>
</template>

<script>
export default {
  props: {
    comment: {
      type: Object,
      default: null,
    },
  },
  computed: {
    image() {
      return (
        this.comment.author.image ||
        'https://cdn.pixabay.com/photo/2020/06/24/19/12/cabbage-5337431_1280.jpg'
      )
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
}
</script>

<style scoped>
.comment {
  padding: 5px;
}

.author {
  text-decoration: none;
}
</style>