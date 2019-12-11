<template>
  <div class="text-center">
    <v-snackbar v-model="flash.snackbar" :timeout="flash.timeout" color="black">
      {{ flash.message }}
      <v-btn color="light" outlined @click="flash.snackbar = false">Close</v-btn>
    </v-snackbar>
  </div>
</template>

<script>
export default {
  name: "Flash",
  props: {
    flash: {
      type: Object,
      default: {
        message: {
          type: String,
          default: "unknown"
        },
        color: {
          type: String,
          default: "error"
        },
        snackbar: {
          type: Boolean,
          default: false
        },
        timeout: {
          type: Number,
          default: 3000
        }
      }
    }
  }
};
</script> 
