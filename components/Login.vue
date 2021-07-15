<template>
  <v-container class="text-center">
    <v-row>
      <v-col>
        <h2>Login</h2>
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
              :type="show ? 'text' : 'password'"
              :append-icon="show ? 'mdi-eye-off' : 'mdi-eye'"
              @click:append="show = !show"
              @keyup.enter="login"
              outlined
              dense
            ></v-text-field>
          </v-list-item>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn outlined text @click="login">Log in</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    email: '',
    password: '',
    show: false,
  }),
  methods: {
    login() {
      console.log('Login button pressed!')
      this.$emit('login')
    }
  },
  mounted(){
    console.log('UwU')
  }
}

// export default {
//   services: {
//     authService,
//   },
//   methods: {
//     async login() {
//       const res = await authService.login(this.email, this.password);
//       localStorage.token = res.data.token;
//       localStorage.id = res.data.id;
//       localStorage.name = res.data.name;
//       localStorage.email = res.data.email;
//       this.$emit("login");
//     },
//   },
// };
</script>
