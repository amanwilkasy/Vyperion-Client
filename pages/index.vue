<template>
  <v-img :aspect-ratio="16/9" :src="require('static/numericWeb.jpg')">
    <v-row align="center" class="lightbox white--text pa-2 fill-height">
      <v-col>
        <v-card color="black">
          <br />
          <v-row align="center" justify="center">
            <v-col class="text-center" cols="12">
              <h1 class="display-3 font-weight-light mb-4">Sensitive Digital Key Manager</h1>
              <h1 class="display-2 font-weight-thin">Key Management API</h1>
            </v-col>
          </v-row>

          <v-card-actions>
            <v-btn large outlined>Learn More</v-btn>
            <v-btn large outlined>Get Started</v-btn>
            <v-spacer />
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-img>
</template>

<script>
export default {
  auth: false,
  data: () => ({}),
  created() {},
  components: {},
  methods: {},
  computed: {},
  mounted() {}
};
</script>

