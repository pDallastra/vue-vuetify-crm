<template>
    <v-navigation-drawer
      v-model="drawer"
      app
    >
      <!--  -->
    </v-navigation-drawer>
</template>

<script>

export default {
  name: 'SideBar',

  data: () => ({
    drawer: false
  }),

  watch: {
    outsideDrawer(value) {
      this.drawer = value;
    }
  },

  props: {
    outsideDrawer: {
      type: Boolean,
      required: true
    }
  },
};
</script>
