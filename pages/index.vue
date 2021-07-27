<template>
  <div>
    <v-carousel
      cycle
      :interval="3600"
      height="200"
      hide-delimiters
      show-arrows-on-hover
    >
      <v-carousel-item
        v-for="(slide, i) in slides"
        :key="i"
        :src="slide.image"
        class="carousel"
        @click="redirect(slide.path)"
      >
        <v-row class="fill-height" align="center" justify="center">
          <div class="slide">{{ slide.description }}</div>
        </v-row>
      </v-carousel-item>
    </v-carousel>
    <v-row class="header">
      <v-col>
        <h1>Browse into amazing ideas and projects</h1>
        <hr />
        <h1>Where do we start?</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-card class="mx-auto" max-width="400" outlined>
          <v-img
            class="white--text align-end"
            height="200px"
            src="http://edap.es/wp-content/uploads/blog9-img-01.jpg"
          >
            <v-card-title class="card2">Projects</v-card-title>
          </v-img>

          <v-card-text class="text--primary">
            <span>Explore new projects or create a new one</span>
          </v-card-text>

          <v-card-actions>
            <v-btn color="orange" to="/projects" block text>
              Exlore projects
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      <v-col>
        <v-card class="mx-auto" max-width="400" outlined>
          <v-img
            class="white--text align-end"
            height="200px"
            src="https://blogs.iadb.org/conocimiento-abierto/wp-content/uploads/sites/10/2019/09/banner-siete-pasos-gestion-proyectos.jpg"
          >
            <v-card-title class="card1">Ideas</v-card-title>
          </v-img>
          <v-card-text class="text--primary">
            <span>Explore new ideas or create a new one</span>
          </v-card-text>

          <v-card-actions>
            <v-btn color="orange" to="/ideas" block text> Explore ideas </v-btn>
          </v-card-actions>
        </v-card>
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
  font-size: 60px;
  background-color: rgba(146, 149, 151, 0.5);
  width: 1161px;
  text-align: center;
  font-family: 'Montserrat', sans-serif;
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
  color: rgb(0, 0, 0);
  font-family: 'Montserrat', sans-serif;
  font-style: normal;
  font-weight: 300;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
  margin-top: 20px;
  margin-bottom: 20px;
}

.card1 {
  background-color: rgba(0, 0, 0, 0.5);
  font-family: 'Montserrat', sans-serif;
}

.card2 {
  background-color: rgba(0, 0, 0, 0.6);
  font-family: 'Montserrat', sans-serif;
}
</style>
