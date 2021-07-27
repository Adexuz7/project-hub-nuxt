<template>
  <v-container>
    <v-row>
      <v-col>
        <v-expansion-panels v-model="open">
          <v-expansion-panel>
            <v-expansion-panel-header>
              <span>Create a new project</span>
            </v-expansion-panel-header>
            <v-expansion-panel-content>
              <v-container>
                <v-row>
                  <v-col>
                    <v-text-field
                      v-model="name"
                      label="Name"
                      outlined
                      dense
                    ></v-text-field>
                    <v-textarea
                      v-model="description"
                      label="Description"
                      no-resize
                      outlined
                      dense
                    ></v-textarea>
                    <v-select
                      v-model="categories"
                      :items="selectCategories"
                      label="Category"
                      item-value="id"
                      item-text="name"
                      return-object
                      outlined
                      dense
                    ></v-select>
                    <v-btn block color="primary" @click="newProject">
                      New project
                    </v-btn>
                  </v-col>
                </v-row>
              </v-container>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  props: {
    selectCategories: {
      type: Array,
      default: null,
    },
  },
  data: () => ({
    valid: true,
    name: '',
    description: '',
    categories: [],
    open: false,
  }),
  methods: {
    async newProject() {
      await this.$axios.$post('/projects', {
        name: this.name,
        description: this.description,
        categories: this.categories,
      })

      this.open = false
      this.$emit('projectCreated')
    },
  },
}
</script>
