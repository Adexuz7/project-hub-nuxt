<template>
  <v-container class="text-center">
    <v-row>
      <v-col>
        <h2>Sign up</h2>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <span>Already registered?</span>
        <v-btn x-small outlined rounded color="secondary" nuxt to="/login">Log in</v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-card class="mx-auto" max-width="400" outlined>
          <v-list-item>
            <!--  -->
          </v-list-item>

          <v-list-item>
            <v-text-field
              v-model="name"
              label="Name"
              outlined
              dense
            ></v-text-field>
          </v-list-item>

          <v-list-item>
            <v-text-field
              v-model="username"
              label="User Name"
              outlined
              dense
            ></v-text-field>
          </v-list-item>

          <v-list-item>
            <v-text-field
              v-model="email"
              label="E-mail"
              outlined
              dense
            ></v-text-field>
          </v-list-item>

          <v-list-item>
            <v-text-field
              v-model="password"
              label="Password"
              outlined
              dense
              :type="show ? 'text' : 'password'"
              :append-icon="show ? 'mdi-eye-off' : 'mdi-eye'"
              @click:append="show = !show"
              @keyup.enter="login"
            ></v-text-field>
          </v-list-item>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn outlined color="primary" @click="signup">Sign up</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    name: '',
    username: '',
    email: '',
    password: '',
    show: false,
    error: null,
  }),
  methods: {
    async signup() {
      try {
        await this.$axios.post('auth/signup', {
          name: this.name,
          userName: this.username,
          email: this.email,
          password: this.password,
        })

        await this.$auth.loginWith('local', {
          data: {
            email: this.email,
            password: this.password,
          },
        })

        this.$router.push('/')
      } catch (e) {
        this.error = e.response.data.message
      }
    },
  },
}
</script>
