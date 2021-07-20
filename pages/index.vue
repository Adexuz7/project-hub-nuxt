<template>
  <div>
    <v-hover v-slot:default="{ hover }">
      <v-carousel
        cycle
        :interval="3500"
        height="200"
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
            <v-overlay v-if="hover" absolute color="#FFE0B2" class="d-flex align-end">
              <v-btn class="mb-12" @click="redirect(slide.path)">See more info</v-btn>
            </v-overlay>
          </v-fade-transition>
        </v-carousel-item>
      </v-carousel>
    </v-hover>
    <v-row class="header">
      <v-col>
        <h1>
          Browse into thousands ideas from +1.000.000 users
        </h1>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  auth: false,
  data() {
    return {
      slides: [
        {
          image:
            'https://blogs.deusto.es/alumni/wp-content/uploads/sites/30/2016/03/bright-idea.jpg',
          description: 'IDEAS',
          path: '/ideas/',
        },
        {
          image: 'https://wallpaperaccess.com/full/1930875.jpg',
          description: 'PROJECTS',
          path: '/projects/',
        },
        {
          image:
            'https://cdn.discordapp.com/attachments/833476896175423505/864544566144008272/Screenshot_from_2021-07-05_15-35-31.png',
          description: 'NEWS',
          path: '/projects/',
        },
        {
          image:
            'https://latabernadelandroidehome.files.wordpress.com/2019/12/wallpaper-engine.jpg?w=816',
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
  color: rgb(255, 255, 255);
  font-size: 70px;
  background-color: rgba(241, 132, 209, 0.5);
  width: 1161px;
  text-align: center;
}

.header {
  text-align: center;
  padding: 30px;
}

.SeeMore {
  justify-content: center;

}

h1 {
  font-size: 40px;
  color: white;
  font-family: 'Montserrat Alternates', sans-serif;
 font-style: normal;
 font-weight: 300;
}

</style>
