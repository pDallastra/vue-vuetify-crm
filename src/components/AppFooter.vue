<template>
  <v-footer 
    fixed
    color="red darken-1 white--text"
  >
    <v-col
      class="text-center"
      cols="12"
    >
      {{ new Date().getFullYear() }} — <strong>Dallastra Tech</strong>
    </v-col>
  </v-footer>
</template>

<script>

export default {
  name: 'AppFooter'
}

</script>