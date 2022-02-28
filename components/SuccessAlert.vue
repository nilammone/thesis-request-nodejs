<template>
  <div>
    <!-- s alert -->
    <v-snackbar v-model="snackbar" absolute bottom color="success" outlined>
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn color="success" text v-bind="attrs" @click="snackbar = false">
          <!-- <v-icon>mdi-close-circle</v-icon> -->
          <v-icon>mdi-check</v-icon>
        </v-btn>
      </template>
    </v-snackbar>
    <!-- e alert -->
  </div>
</template>

<script>
export default {
  props: ['snackbar'],
  data() {
    return {
      text: 'Success!.',
    }
  },
}
</script>

<style></style>
