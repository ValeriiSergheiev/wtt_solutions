<template>
  <div class="mt-3">
    <v-data-table
      v-model="selected"
      :headers="headers"
      :items="desserts"
      item-key="name"
      :items-per-page="5"
      show-select
      class="elevation-1"
    >
      <template v-slot:footer>
        <v-btn class="ma-2" color="primary" @click="addItem">
          Add dessert
        </v-btn>
        <v-btn
          class="ma-2"
          color="warning"
          :disabled="isActiveBtn"
          @click="editItem()"
        >
          Edit
        </v-btn>
        <v-btn class="ma-2" color="error" @click="deleteItem">
          Delete
        </v-btn>
      </template>
    </v-data-table>
    <template>
      <v-row justify="center">
        <v-dialog v-model="dialog" persistent max-width="600px">
          <v-card>
            <v-card-title>
              <span class="headline">{{ modalTitle }} Dessert</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12">
                    <v-text-field
                      v-model="formData.name"
                      label="Dessert (100g serving)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model.number="formData.calories"
                      label="Calories"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model.number="formData.fat"
                      label="Fat (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model.number="formData.carbs"
                      label="Carbs (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model.number="formData.protein"
                      label="Protein (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="formData.iron"
                      :items="['0%', '25%', '50%', '75%']"
                      label="Iron (%)"
                    ></v-select>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="dialog = false">
                Close
              </v-btn>
              <v-btn color="blue darken-1" text @click="save">
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </template>
  </div>
</template>

<script>
export default {
  name: 'ListDesserts',
  data: () => ({
    selected: [],
    selectedIndex: [],
    headers: [
      {
        text: 'Dessert (100g serving)',
        align: 'start',
        sortable: false,
        value: 'name'
      },
      { text: 'Calories', value: 'calories' },
      { text: 'Fat (g)', value: 'fat' },
      { text: 'Carbs (g)', value: 'carbs' },
      { text: 'Protein (g)', value: 'protein' },
      { text: 'Iron (%)', value: 'iron' }
    ],
    dessertsJSON: [
      {
        name: 'Frozen Yogurt',
        calories: 159,
        fat: 6.0,
        carbs: 24,
        protein: 4.0,
        iron: '1%'
      },
      {
        name: 'Ice cream sandwich',
        calories: 237,
        fat: 9.0,
        carbs: 37,
        protein: 4.3,
        iron: '1%'
      },
      {
        name: 'Eclair',
        calories: 262,
        fat: 16.0,
        carbs: 23,
        protein: 6.0,
        iron: '7%'
      },
      {
        name: 'Cupcake',
        calories: 305,
        fat: 3.7,
        carbs: 67,
        protein: 4.3,
        iron: '8%'
      },
      {
        name: 'Gingerbread',
        calories: 356,
        fat: 16.0,
        carbs: 49,
        protein: 3.9,
        iron: '16%'
      },
      {
        name: 'Jelly bean',
        calories: 375,
        fat: 0.0,
        carbs: 94,
        protein: 0.0,
        iron: '0%'
      },
      {
        name: 'Lollipop',
        calories: 392,
        fat: 0.2,
        carbs: 98,
        protein: 0,
        iron: '2%'
      },
      {
        name: 'Honeycomb',
        calories: 408,
        fat: 3.2,
        carbs: 87,
        protein: 6.5,
        iron: '45%'
      },
      {
        name: 'Donut',
        calories: 452,
        fat: 25.0,
        carbs: 51,
        protein: 4.9,
        iron: '22%'
      },
      {
        name: 'KitKat',
        calories: 518,
        fat: 26.0,
        carbs: 65,
        protein: 7,
        iron: '6%'
      }
    ],
    desserts: [],
    dialog: false,
    modalTitle: '',
    formData: {
      name: '',
      calories: '',
      fat: '',
      carbs: '',
      protein: '',
      iron: ''
    }
  }),
  mounted() {
    this.desserts = this.dessertsJSON
  },
  watch: {
    selected: function() {
      this.selectedIndex = []
      this.selected.forEach((s) => {
        this.selectedIndex.push(this.desserts.indexOf(s))
      })
    }
  },
  methods: {
    addItem() {
      this.selected = []
      this.dialog = true
    },
    editItem() {
      this.formData = Object.assign({}, this.selected[0])
      this.dialog = true
    },
    deleteItem() {
      this.desserts = this.desserts.filter(
        (f) => this.selected.indexOf(f) === -1
      )
      this.selectedIndex = []
      this.selected = []
    },
    save() {
      if (this.selectedIndex.length === 1) {
        Object.assign(this.desserts[this.selectedIndex[0]], this.formData)
      } else {
        this.desserts.unshift(this.formData)
      }
      this.dialog = false
      this.selected = []
      this.formData = {
        name: '',
        calories: '',
        fat: '',
        carbs: '',
        protein: '',
        iron: ''
      }
    }
  },
  computed: {
    isActiveBtn() {
      return this.selected.length !== 1
    }
  }
}
</script>

<style scoped></style>
