<template>
  <v-app>
    <v-main>
      <div>
        <v-card max-width="700" class="mx-auto">
          <v-app-bar dark color="#ed1c27">
            <v-toolbar-title>Pagos En Linea</v-toolbar-title>
            <v-spacer></v-spacer>
          </v-app-bar>
          <v-container> <Products /></v-container>
        </v-card>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import Products from './components/Products'
export default {
  name: 'App',

  components: {
    Products
  },

  data: () => ({
    //
  })
}
</script>
