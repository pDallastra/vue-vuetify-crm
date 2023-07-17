<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      app
    >
      <!--  -->
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Application</v-toolbar-title>
    </v-app-bar>

    <TableData/>

  </v-app>
</template>

<script>
import TableData from './components/TableData.vue';


export default {
  name: 'App',

  components: { TableData },

  data: () => ({
    drawer: false,
  })
};
</script>
