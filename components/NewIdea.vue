<template>
  <v-container>
    <v-row>
      <v-col>
        <v-expansion-panels color="pink" v-model="open">
          <v-expansion-panel>
            <v-expansion-panel-header>
              <span>Create a new idea</span>
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
                  </v-col>
                </v-row>
                <v-row>
                  <v-col>
                    <v-text-field
                      v-model="description"
                      label="Description"
                      outlined
                      dense
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col class="d-flex">
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
                  </v-col>
                </v-row>
                <v-row>
                  <v-col class="d-flex">
                    <v-select
                      v-model="difficulty"
                      :items="selectDifficulty"
                      label="Difficulty"
                      outlined
                      dense
                    ></v-select>
                  </v-col>
                </v-row>
                <v-row>
                  <v-btn color="primary" class="mr-4" @click="newIdea">
                    New idea
                  </v-btn>
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
  name: 'NewIdea',
  props: {
    selectCategories: {
      type: Array,
      default: null,
    },
  },
  data: () => ({
    name: '',
    description: '',
    categories: [],
    difficulty: '',
    selectDifficulty: ['Easy', 'Medium', 'Difficult'],
    open: false,
  }),
  methods: {
    async newIdea() {
      await this.$axios.$post('/ideas', {
        name: this.name,
        description: this.description,
        categories: this.categories,
        difficulty: this.difficulty,
      })

      this.open = false
      this.$emit('ideaCreated')
    },
  },
}
</script>
