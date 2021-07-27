<template>
  <v-container>
    <v-row justify="space-around">
      <v-card width="400" height="600">
        <v-img
          height="200px"
          src="https://steamuserimages-a.akamaihd.net/ugc/99473989857590016/F8DBF4DAFCD7A536377BC48764242B4FBEFBC7C6/?imw=512&imh=454&ima=fit&impolicy=Letterbox&imcolor=%23000000&letterbox=true"
        >
          <v-app-bar flat color="rgba(0, 0, 0, 0)">
            <v-app-bar-nav-icon color="white"></v-app-bar-nav-icon>

            <v-toolbar-title class="text-h6 white--text pl-0">
              Messages
            </v-toolbar-title>

            <v-spacer></v-spacer>

            <v-btn color="white" icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-app-bar>

          <v-card-title class="white--text mt-8">
            <v-avatar size="56">
              <img
                alt="user"
                src="https://pbs.twimg.com/media/Dweca4yU0AAYZUv?format=jpg&name=small"
              />
            </v-avatar>
            <p class="ml-3">
              {{ userTeams }}
            </p>
          </v-card-title>
        </v-img>

        <v-card-text>
          <div class="font-weight-bold ml-8 mb-2">Today</div>

          <v-timeline align-top dense>
            <v-timeline-item
              v-for="message in messages"
              :key="message.time"
              :color="message.color"
              small
            >
              <div>
                <div class="font-weight-normal"></div>
              </div>
            </v-timeline-item>
          </v-timeline>
        </v-card-text>
      </v-card>
    </v-row>
  </v-container>
</template>

<script>
export default {
  props: {
    team: {
      type: Object,
      default: null,
    },
  },
}
</script>


