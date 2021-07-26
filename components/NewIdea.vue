<template>
  <v-container>
    <v-row>
      <v-col>
        <v-expansion-panels v-model="open">
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
                    <v-select
                      v-model="difficulty"
                      :items="selectDifficulty"
                      label="Difficulty"
                      outlined
                      dense
                    ></v-select>
                    <v-btn
                    :disabled="!valid"
                    block color="primary"
                    @click="newIdea"> New idea </v-btn>
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
  name: 'NewIdea',
  props: {
    selectCategories: {
      type: Array,
      default: null,
    },
  },
  data: () => ({
    valid: false,
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
