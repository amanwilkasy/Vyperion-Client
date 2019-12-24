<template>
  <v-container>
    <ValidationObserver v-slot="{ invalid }">
      <v-form @submit.prevent="submit">
        <validation-provider rules="required" v-slot="{ errors }">
          <v-text-field
            v-model="signUpUser.firstName"
            placeholder="John"
            label="First Name"
            type="text"
            append-icon="fas fa-user"
            name="First Name"
            clearable
            required
          />
          <span>{{ errors[0] }}</span>
        </validation-provider>

        <validation-provider rules="required" v-slot="{ errors }">
          <v-text-field
            v-model="signUpUser.lastName"
            placeholder="Doe"
            label="Last Name"
            type="text"
            append-icon="fas fa-user"
            name="Last Name"
            clearable
            required
          />
          <span>{{ errors[0] }}</span>
        </validation-provider>

        <validation-provider rules="required" v-slot="{ errors }">
          <v-text-field
            v-model="signUpUser.email"
            placeholder="johndoe@email.com"
            label="Email"
            type="email"
            append-icon="fas fa-envelope"
            name="email"
            clearable
            required
          />
          <span>{{ errors[0] }}</span>
        </validation-provider>

        <validation-provider rules="required" v-slot="{ errors }">
          <v-text-field
            v-model="signUpUser.password"
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

        <v-btn color="primary" :disabled="invalid" type="submit">Sign Up</v-btn>
      </v-form>
    </ValidationObserver>
    <Flash :flash="flash" />
  </v-container>
</template>

<script>
import { ValidationProvider, ValidationObserver } from "vee-validate";
import { ENDPOINTS } from "./endpoints";
export default {
  name: "SignUp",
  auth: false,
  data: () => ({
    signUpUser: {
      type: "signUpUser",
      firstName: "",
      lastName: "",
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
  components: {
    ValidationProvider,
    ValidationObserver,
    Flash: () => import("~/components/Flash")
  },
  methods: {
    submit() {
      this.$axios
        .post(ENDPOINTS.SignUp, this.signUpUser)
        .then(() => {
          this.signIn();
        })
        .catch(error => this.flashMessage(error.message, "error"));
    },
    signIn() {
      this.$auth
        .login({
          data: {
            type: "signInUser",
            email: this.signUpUser.email,
            password: this.signUpUser.password
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
        setTimeout(() => location.reload(), 800);
      }
    }
  }
};
</script>
<style scoped>
span {
  display: block;
}
</style>
