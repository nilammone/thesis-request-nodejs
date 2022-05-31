<template>
  <div class="mt-3">
    <v-card>
      <v-toolbar dark flat>
        <template v-slot:extension>
          <v-tabs v-model="tab" align-with-title>
            <v-tabs-slider color="yellow"></v-tabs-slider>

            <v-tab> Node Js </v-tab>
            <v-tab> Java Spring boot </v-tab>
          </v-tabs>
        </template>
      </v-toolbar>

      <v-tabs-items v-model="tab">
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <nodejs />
            </v-card-text>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <javasp />
            </v-card-text>
          </v-card>
        </v-tab-item>
      </v-tabs-items>
    </v-card>
  </div>
</template>

<script>
import nodejs from '~/components/NodeJs.vue'
import javasp from '~/components/JavaSP.vue'
export default {
  components: {
    nodejs,
    javasp,
  },
  data() {
    return {
      tab: null,
    }
  },
}
</script>

<style scoped>
.chbg {
  background-color: white;
}

.myft10 {
  font-size: 1em;
}
</style>
