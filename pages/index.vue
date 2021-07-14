<template>
  <div>
    <v-row class="header">
      <v-col>
        <h1>WELCOME</h1>
      </v-col>
    </v-row>
    <v-hover v-slot:default="{ hover }">
      <v-carousel
        cycle
        :interval="3500"
        height="400"
        hide-delimiter-background
        show-arrows-on-hover
      >
        <v-carousel-item
          v-for="(slide, i) in slides"
          :key="i"
          :src="slide.image"
          class="carousel"
        >
          <v-row class="fill-height" align="center" justify="center">
            <div class="slide">{{ slide.description }}</div>
          </v-row>
          <v-fade-transition>
            <v-overlay v-if="hover" absolute color="#FFE0B2">
              <v-btn @click="redirect(slide.path)">See more info</v-btn>
            </v-overlay>
          </v-fade-transition>
        </v-carousel-item>
      </v-carousel>
    </v-hover>
  </div>
</template>



<script>
export default {
  data() {
    return {
      slides: [
        {
          image: 'https://images4.alphacoders.com/640/thumbbig-640956.webp',
          description: 'IDEAS',
          path: '/ideas/',
        },
        {
          image:
            'https://cdn.discordapp.com/attachments/833476896175423505/864544544296796180/67343.jpg',
          description: 'projects',
          path: '/projects/',
        },
        {
          image:
            'https://cdn.discordapp.com/attachments/833476896175423505/864544566144008272/Screenshot_from_2021-07-05_15-35-31.png',
          description: 'projects',
          path: '/projects/',
        },
        {
          image:
            'https://cdn.discordapp.com/attachments/833476896175423505/864544627091439626/Screenshot_from_2021-07-08_17-37-28.png',
          description: 'PROJECTS',
          path: '/projects/',
        },
      ],
      overlay: false,
    }
  },
  methods: {
    redirect(path) {
      this.$router.push(path)
    },
  },
}
</script>

<style scoped>
.slide {
  color: white;
  text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000,
    1px 1px 0 #000;
  font-size: 100px;
}

.header {
  text-align: center;
  padding: 30px;
}
</style>