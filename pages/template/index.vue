<template>
  <v-layout column justify-center align-center>
    <h1>index page</h1>

    <div v-if="$auth.loggedIn">
      logged in
      {{$auth.user}}
      <v-btn @click="signOut">Log out</v-btn>
      <v-btn @click="getData">Get Data</v-btn>
      <!-- {{$auth.strategies}} -->
    </div>

    <div v-else>not logged in</div>
  </v-layout>
</template>

<script>
import Logo from "~/components/Logo.vue";
import VuetifyLogo from "~/components/VuetifyLogo.vue";

export default {
  methods: {
    signOut() {
      this.$auth
        .logout()
        .then(() => {
          this.$router.push("/login");
        })
        .catch(error => {
          console.log(error);
        });
    },
    getData() {
      this.$axios
        .get("apikey/402881706e907115016e9071202d0000")
        .then(res => {
          console.log(res.data);
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  components: {
    Logo,
    VuetifyLogo
  }
};
</script>
