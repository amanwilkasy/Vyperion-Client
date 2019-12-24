<template>
  <v-app dark>
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.8.2/css/all.min.css"
      integrity="sha384-oS3vJWv+0UjzBfQzYUhtDYW+Pj2yciDJxpsK1OYPAYjqT085Qq/1cq5FLXAZQ7Ay"
      crossorigin="anonymous"
    />
    <Navigation/>
    <v-content>
      <v-container fluid>
        <nuxt/>
      </v-container>
    </v-content>
    <Footer/>
  </v-app>
</template>

<script>
  export default {
    components: {
      Navigation: () => import("~/components/Navigation"),
      Footer: () => import("~/components/Footer")
    }
  };
</script>
