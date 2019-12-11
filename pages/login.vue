<template>
  <v-container>
    <ValidationObserver v-slot="{ invalid }">
      <form @submit.prevent="submit">
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
          <!--type="email"-->
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
        <v-btn :disabled="invalid" type="submit">Login</v-btn>
        <Flash :flash="flash" />
      </form>
    </ValidationObserver>
  </v-container>
</template>

<script>
import { ENDPOINTS } from "./endpoints";
import { ValidationProvider, ValidationObserver } from "vee-validate";
export default {
  name: "Navigation",
  data: () => ({
    user: {
      type: "signInUser",
      email: "",
      password: ""
    },
    flash: {
      message: "",
      color: "",
      snackbar: false,
      timeout: 3000
    }
  }),
  methods: {
    submit() {
      this.$auth
        .login({
          data: {
            type: "signInUser",
            email: this.user.email,
            password: this.user.password
          }
        })
        .then(() => {
          this.flashMessage("Logged In", "success");
          this.$router.push("/dashboard");
        })
        .catch(error => this.flashMessage(error.message, "error"));
    },
    flashMessage(message, color, reload) {
      this.flash.message = message;
      this.flash.color = color;
      this.flash.snackbar = true;
      if (reload) {
        setTimeout(() => location.reload(), 3000);
      }
    }
  },
  components: {
    ValidationProvider,
    ValidationObserver,
    Flash: () => import("~/components/Flash")
  }
};
</script>

<style scoped>
span {
  display: block;
}
</style>
