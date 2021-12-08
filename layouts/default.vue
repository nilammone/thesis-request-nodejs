<template>
  <v-app>
    <!-- menu left size -->
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exactc
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar class="mywhite" :clipped-left="clipped" fixed app>
      <!-- button menu -->
      <v-app-bar-nav-icon class="mygrey" @click.stop="drawer = !drawer" />

      <v-toolbar-title v-text="title" style="color: black" />
      <v-spacer />
    </v-app-bar>
    <!-- main space -->
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <!-- footer -->
    <v-footer class="mywhite" :absolute="!fixed" app>
      <span style="color: black">&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire',
        },
      ],
      miniVariant: false,
      title: 'Asset System',
    }
  },
}
</script>
