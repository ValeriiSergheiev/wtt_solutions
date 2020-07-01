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
                      :error-messages="nameErrors"
                      :counter="10"
                      required
                      @input="$v.name.$touch()"
                      @blur="$v.name.$touch()"
                      label="Dessert (100g serving)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="formData.calories"
                      label="Calories"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="formData.fat"
                      label="Fat (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="formData.carbs"
                      label="Carbs (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="formData.protein"
                      label="Protein (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="8">
                    <v-text-field
                      v-model="formData.email"
                      :error-messages="emailErrors"
                      label="E-mail"
                      required
                      @input="$v.email.$touch()"
                      @blur="$v.email.$touch()"
                    ></v-text-field>
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
import { validationMixin } from 'vuelidate'
import { required, maxLength, email } from 'vuelidate/lib/validators'

export default {
  mixins: [validationMixin],
  validations: {
    name: { required, maxLength: maxLength(10) },
    email: { required, email }
  },
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
      { text: 'Email', value: 'email' }
    ],
    dessertsJSON: [
      {
        name: 'Frozen Yogurt',
        calories: 159,
        fat: 6.0,
        carbs: 24,
        protein: 4.0,
        email: 'email1@test.com'
      },
      {
        name: 'Ice cream sandwich',
        calories: 237,
        fat: 9.0,
        carbs: 37,
        protein: 4.3,
        email: 'email2@test.com'
      },
      {
        name: 'Eclair',
        calories: 262,
        fat: 16.0,
        carbs: 23,
        protein: 6.0,
        email: 'email3@test.com'
      },
      {
        name: 'Cupcake',
        calories: 305,
        fat: 3.7,
        carbs: 67,
        protein: 4.3,
        email: 'email4@test.com'
      },
      {
        name: 'Gingerbread',
        calories: 356,
        fat: 16.0,
        carbs: 49,
        protein: 3.9,
        email: 'email5@test.com'
      },
      {
        name: 'Jelly bean',
        calories: 375,
        fat: 0.0,
        carbs: 94,
        protein: 0.0,
        email: 'email6@test.com'
      },
      {
        name: 'Lollipop',
        calories: 392,
        fat: 0.2,
        carbs: 98,
        protein: 0,
        email: 'email7@test.com'
      },
      {
        name: 'Honeycomb',
        calories: 408,
        fat: 3.2,
        carbs: 87,
        protein: 6.5,
        email: 'email8@test.com'
      },
      {
        name: 'Donut',
        calories: 452,
        fat: 25.0,
        carbs: 51,
        protein: 4.9,
        email: 'email9@test.com'
      },
      {
        name: 'KitKat',
        calories: 518,
        fat: 26.0,
        carbs: 65,
        protein: 7,
        email: 'email10@test.com'
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
      email: ''
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
      this.$v.$reset()
      this.selected = []
      this.modalTitle = 'Add'
      this.dialog = true
    },
    editItem() {
      this.formData = Object.assign({}, this.selected[0])
      this.modalTitle = 'Edit'
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
      this.$v.$touch()
      if (!this.$v.$invalid) {
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
          email: ''
        }
      }
    }
  },
  computed: {
    isActiveBtn() {
      return this.selected.length !== 1
    },
    nameErrors() {
      const errors = []
      if (!this.$v.name.$dirty) return errors
      !this.$v.name.maxLength &&
        errors.push('Dessert must be at most 10 characters long')
      !this.$v.name.required && errors.push('Dessert is required.')
      return errors
    },
    emailErrors() {
      const errors = []
      if (!this.$v.email.$dirty) return errors
      !this.$v.email.email && errors.push('Must be valid e-mail')
      !this.$v.email.required && errors.push('E-mail is required')
      return errors
    },
    name() {
      return this.formData.name
    },
    email() {
      return this.formData.email
    }
  }
}
</script>

<style scoped></style>
