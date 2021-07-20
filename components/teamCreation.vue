<template>
  <v-container>
    <v-row>
      <v-col>
        <v-expansion-panels>
          <v-expansion-panel>
            <v-expansion-panel-header> Add Team </v-expansion-panel-header>
            <v-expansion-panel-content>
              <v-form v-model="valid">
                <v-container>
                  <v-row>
                    <v-col>
                      <v-text-field
                        v-model="teamName"
                        label="Team name"
                        :rules="nameRules"
                        outlined
                        dense
                        required
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col>
                      <v-textarea
                        v-model="description"
                        label="Team description"
                        outlined
                        auto-grow
                        dense
                        counter
                      >
                      </v-textarea>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col>
                      <v-btn
                        :disabled="!valid"
                        color="primary"
                        class="mr-4"
                        block
                        @click="submit"
                      >
                        Create Team
                      </v-btn>
                    </v-col>
                  </v-row>
                </v-container>
              </v-form>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    valid: false,
    teamName: '',
    nameRules: [(v) => !!v || 'Name is required'],
    description: '',
    members: [],
  }),
  methods: {
    async submit() {
      await this.$axios.post('/teams', {
        name: this.teamName,
        description: this.description,
        members: this.members,
        projects: [],
        messages: [],
      })
    },
  },
}
</script>