<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <h2>Test</h2>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/ValeriiSergheiev/wtt_solutions"
        target="_blank"
        text
      >
        <span class="mr-2">GitHub repository</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-container>
        <ListDesserts />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import ListDesserts from './components/ListDesserts'

export default {
  name: 'App',

  components: {
    ListDesserts
  },

  data: () => ({
    //
  })
}
</script>
