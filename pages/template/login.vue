<template>
  <v-container>
    <ValidationObserver v-slot="{ invalid }">
      <form @submit.prevent="signIn">
        <validation-provider rules="required" v-slot="{ errors }">
          <v-text-field
            v-model="user.email"
            placeholder="johndoe@email.com"
            label="Email"
            append-icon="fas fa-envelope"
            name="email"
            clearable
            required
            autofocus
          />
          <!--          type="email"-->
          <span>{{ errors[0] }}</span>
        </validation-provider>

        <validation-provider rules="required" v-slot="{ errors }">
          <v-text-field
            v-model="user.password"
            placeholder="********"
            label="Password"
            type="password"
            append-icon="fas fa-key"
            name="password"
            clearable
            required
          />
          <span>{{ errors[0] }}</span>
        </validation-provider>
        <v-btn color="primary" :disabled="invalid" type="submit">Login</v-btn>
      </form>
    </ValidationObserver>

    <div v-if="$auth.loggedIn">
      logged in
      {{$auth.user}}
      <v-btn @click="$auth.logout()">Log out</v-btn>
      <!-- {{$auth.strategies}} -->
    </div>

    <div v-else>not logged in</div>
  </v-container>
</template>

<script>
import { ValidationProvider, ValidationObserver } from "vee-validate";
export default {
  name: "Login",
  data: () => ({
    user: {
      email: "",
      password: ""
    }
  }),
  methods: {
    signIn() { 

      this.$auth
        .login({
          data: {
            type: "signInUser",
            email: this.user.email,
            password: this.user.password
          }
        })
        .then(() => { 
          this.$router.push("/");
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  components: {
    ValidationProvider,
    ValidationObserver
  }
};
</script>

<style scoped>
span {
  display: block;
}
</style>
