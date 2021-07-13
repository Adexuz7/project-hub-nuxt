<template>
  <v-carousel
    cycle
    :interval="3500"
    height="400"
    hide-delimiter-background
    show-arrows-on-hover
  >
    <v-carousel-item v-for="(slide, i) in slides" :key="i" :src="images[i].src">
      <v-row class="fill-height" align="center" justify="center">
        <div class="text-h2">{{ slide }}</div>
      </v-row>
    </v-carousel-item>
  </v-carousel>
</template>

<script>
export default {
  data() {
    return {
      images: [
        {
          src: 'https://images4.alphacoders.com/640/thumbbig-640956.webp',
        },
        {
          src: 'https://cdn.discordapp.com/attachments/833476896175423505/864544544296796180/67343.jpg',
        },
        {
          src: 'https://cdn.discordapp.com/attachments/833476896175423505/864544566144008272/Screenshot_from_2021-07-05_15-35-31.png',
        },
        {
          src: 'https://cdn.discordapp.com/attachments/833476896175423505/864544627091439626/Screenshot_from_2021-07-08_17-37-28.png',
        },
      ],
      slides: ['NEW SERIES', 'NEW WALLPAPERS', 'NEW MEETINGS', 'NEW GAMES'],
    }
  },
}
</script>