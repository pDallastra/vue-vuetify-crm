<template>
  <v-app>
    <SideBar :outside-drawer="drawer"/>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Application</v-toolbar-title>
    </v-app-bar>

    <v-main>      
      <TableData/>
    </v-main>

  </v-app>
</template>

<script>
import SideBar from './components/Sidebar.vue';
import TableData from './components/TableData.vue';


export default {
  name: 'App',

  components: { TableData, SideBar },

  data: () => ({
    drawer: false,
  })
};
</script>
