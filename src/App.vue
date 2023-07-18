<template>
  <v-app>
    <SideBar :outside-drawer="drawer"/>

    <v-app-bar 
      app 
      color="red darken-1 white--text"
    >
      <v-app-bar-nav-icon 
        @click="drawer = !drawer" 
        color="white"
      />

      <v-toolbar-title>Application</v-toolbar-title>
    </v-app-bar>

    <v-main>      
      <TableData/>
    </v-main>

    <AppFooter/>

  </v-app>
</template>

<script>
import AppFooter from './components/AppFooter.vue';
import SideBar from './components/Sidebar.vue';
import TableData from './components/TableData.vue';


export default {
  name: 'App',

  components: { TableData, SideBar, AppFooter },

  data: () => ({
    drawer: false,
  })
};
</script>
