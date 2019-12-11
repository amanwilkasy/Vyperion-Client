<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <template v-slot:activator="{ on }">
      <v-btn outlined v-on="on">{{name}}</v-btn>
    </template>
    <form @submit.prevent="submitRequest">
      <v-card>
        <v-card-title>
          <span class="headline">{{name}} Api Key</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field label="Api Key Name*" v-model="apiKey.keyName" required></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field label="Api Key Value*" v-model="apiKey.keyValue" required></v-text-field>
              </v-col>
            </v-row>
          </v-container>
          <small>*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <div class="flex-grow-1"></div>
          <v-btn outlined @click="dialog = false">Close</v-btn>
          <v-btn outlined type="submit">Submit</v-btn>
        </v-card-actions>
      </v-card>
    </form>
  </v-dialog>
</template>

<script>
export default {
  name: "ApiKeyBtn",
  props: {
    name: {
      type: String,
      required: true,
      default: "unknown"
    },
    keyId: {
      type: Number,
      default: 0
    }
    // userId: {
    //   type: String,
    //   default: ""
    // }
  },
  data: () => ({
    dialog: false,
    apiKey: {
      type: "clientApiKey",
      id: 0,
      keyName: "",
      keyValue: "",
      userId: ""
    }
  }),
  methods: {
    submitRequest() {
      this.apiKey.id = this.$props.keyId;
      this.apiKey.userId = this.$auth.user.id;
      this.$emit("submitRequest", this.apiKey);
      this.dialog = false;
    }
  }
};
</script>
