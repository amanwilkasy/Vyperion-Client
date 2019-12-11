<template>
  <v-container fluid grid-list-lg>
    <v-toolbar color="black">
      <v-toolbar-title>Dashboard</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-toolbar-items>
        <v-btn icon @click.stop="addKeyDialog = true">
          <v-icon>mdi-plus-circle</v-icon>
        </v-btn> 
      </v-toolbar-items>
    </v-toolbar>

    <v-layout row wrap>
      <v-flex xs12 sm4 v-for="key in apiKeys" :key="key.id">
        <v-hover>
          <v-card slot-scope="{ hover }" :class="`elevation-${hover ? 24 : 2}`" color="black">
            <v-card-title>{{key.keyName}}</v-card-title>
            <v-card-text>{{key.keyValue}}</v-card-text>
            <v-card-actions>
              <v-spacer />
              <v-btn icon v-clipboard:copy="key.keyValue" v-clipboard:success="copySuccess">
                <v-icon>mdi-content-copy</v-icon>
              </v-btn> 

              <v-btn icon @click.stop="updateKeyDialog = true; apiKey.id = key.id;">
                <v-icon>mdi-account-edit</v-icon>
              </v-btn>

              <v-btn icon @click="deleteKey(key.id)">
                <v-icon>mdi-delete</v-icon>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-hover>
      </v-flex>
    </v-layout>
    <v-dialog v-model="addKeyDialog" persistent max-width="600px">
      <form @submit.prevent="addKey">
        <v-card>
          <v-card-title>
            <span class="headline">Add Api Key</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field label="Api Key Name*" v-model="apiKey.keyName" required autofocus></v-text-field>
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
            <v-btn outlined @click="addKeyDialog = false">Close</v-btn>
            <v-btn outlined type="submit" @click="addKeyDialog = false">Submit</v-btn>
          </v-card-actions>
        </v-card>
      </form>
    </v-dialog>

    <v-dialog v-model="updateKeyDialog" persistent max-width="600px">
      <form @submit.prevent="updateKey">
        <v-card>
          <v-card-title>
            <span class="headline">Update Api Key</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field label="Api Key Name*" v-model="apiKey.keyName" required autofocus></v-text-field>
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
            <v-btn outlined @click="updateKeyDialog = false">Close</v-btn>
            <v-btn outlined type="submit" @click="updateKeyDialog = false">Submit</v-btn>
          </v-card-actions>
        </v-card>
      </form>
    </v-dialog> 
    <Flash :flash="flash" />
  </v-container>
</template>

<script>
import { ENDPOINTS } from "./endpoints";

export default {
  name: "Dashboard",
  data: () => ({
    addKeyDialog: false,
    updateKeyDialog: false,
    apiKeys: [],
    apiKey: {
      type: "clientApiKey",
      id: 0,
      keyName: "",
      keyValue: ""
    },
    flash: {
      message: "",
      color: "",
      snackbar: false,
      timeout: 3000
    }
  }),
  methods: { 
    getApiKeys() {
      this.$axios
        .get(ENDPOINTS.ApiKey)
        .then(res => (this.apiKeys = res.data))
        .catch(error => this.flashMessage(error.message, "error", false));
    },
    addKey() {
      this.$axios
        .post(ENDPOINTS.ApiKey, this.apiKey)
        .then(res => this.flashMessage(res.data, "success", true))
        .catch(error => this.flashMessage(error.message, "error", false));
    },
    updateKey(key) {
      this.$axios
        .put(ENDPOINTS.ApiKey, this.apiKey)
        .then(res => this.flashMessage(res.data, "success", true))
        .catch(error => this.flashMessage(error.message, "error", false));
    },
    deleteKey(inputId) {
      this.$axios
        .delete(ENDPOINTS.ApiKey.concat(inputId))
        .then(res => {
          this.flashMessage(res.data, "success", true);
        })
        .catch(error => this.flashMessage(error.message, "error", false));
    },
    copySuccess() {
      this.flashMessage("Copied", "success", false);
    },
    flashMessage(message, color, reload) {
      this.flash.message = message;
      this.flash.color = color;
      this.flash.snackbar = true;
      if (reload) {
        setTimeout(() => location.reload(), 800);
      }
    }
  },
  created() {
    this.getApiKeys();
  },
  computed: {},
  components: {
    ApiKeyBtn: () => import("~/components/ApiKeyBtn"),
    Flash: () => import("~/components/Flash")
  }
};
</script>

