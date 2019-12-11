<template>
  <v-footer :fixed="fixed" app>
    <span>&copy; {{year}} {{title}}</span>
    <!-- <span>{{title}}</span> -->
    <v-spacer></v-spacer>

    <v-btn icon>
      <v-icon>mdi-heart</v-icon>
    </v-btn>
    <v-btn icon>
      <v-icon>mdi-share-variant</v-icon>
    </v-btn>
  </v-footer>
</template>

<script>
export default {
  name: "Footer",
  data: () => ({
    fixed: false,
    year: new Date().getFullYear(),
    title: "Vyperion"
  })
};
</script>
