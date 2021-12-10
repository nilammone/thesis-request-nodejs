<template>
  <div>
    <v-main class="bgimg">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
.bgimg {
  background-image: url('/login-bg3.jpg');
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
}
</style>
