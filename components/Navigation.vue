<template>
  <div>
    <v-navigation-drawer v-model="drawer" :mini-variant="miniVariant" :clipped="clipped" fixed app>
      <v-list>
        <div v-if="$auth.loggedIn">
          <v-list-item v-for="(item, i) in loggedInItems" :key="i" :to="item.to" router exact>
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="item.title" />
            </v-list-item-content>
          </v-list-item>
        </div>

        <div v-else>
          <v-list-item v-for="(item, i) in items" :key="i" :to="item.to" router exact>
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="item.title" />
            </v-list-item-content>
          </v-list-item>
        </div>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />

      <v-toolbar-title>
        <nuxt-link to="/">
          <v-img :src="require('static/logo.svg')" width="160"></v-img>
        </nuxt-link>
      </v-toolbar-title>

      <v-spacer />

      <div class="text-center">
        <div v-if="$auth.loggedIn"> 
          <v-btn @click="$auth.logout()">Log out</v-btn>
        </div>

        <div v-else class="hidden-sm-and-down">
          <v-btn to="/signup">Sign Up</v-btn>
          <v-btn to="/login">Log In</v-btn>
        </div>
      </div>
    </v-app-bar>
  </div>
</template>

<script>
export default {
  name: "Navigation",
  data: () => ({
    items: [
      {
        icon: "mdi-apps",
        title: "Home",
        to: "/"
      },
      {
        icon: "mdi-view-dashboard",
        title: "Log In",
        to: "/login"
      },
      {
        icon: "mdi-view-dashboard",
        title: "Sign Up",
        to: "/signup"
      }
    ],
    loggedInItems: [
      {
        icon: "mdi-view-dashboard",
        title: "Dashboard",
        to: "/dashboard"
      }
    ],
    clipped: false,
    drawer: false,
    fixed: false,
    miniVariant: false,
    right: true,
    rightDrawer: false,
    title: "Vyperion"
  }),
  methods: {
    signOut() {
      this.$auth
        .logout()
        .then(() => {
          this.$router.push("/login");
        })
        .catch(error => alert(error));
    }
  },
  computed: {}
};
</script>

<style scoped> 
</style>
