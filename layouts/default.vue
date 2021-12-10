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
        <v-list-item to="/" router exactc>
          <v-list-item-action>
            <v-icon> mdi-home </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>ຫນ້າຫລັກ</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-list>
        <v-list-item to="#" router exactc>
          <v-list-item-action>
            <v-icon> mdi-cog-outline </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>ບໍລິຫານຈັດການ</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <hr />

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

      <v-list>
        <v-list-item to="#" router exactc>
          <v-list-item-action>
            <v-icon> mdi-chart-areaspline </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>ລາຍງານ</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <hr />

      <v-list>
        <v-list-item
          v-for="(itemrp, j) in itemrps"
          :key="j"
          :to="itemrp.to"
          router
          exactc
        >
          <v-list-item-action>
            <v-icon>{{ itemrp.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="itemrp.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-list>
        <v-list-item to="#" router exactc>
          <v-list-item-action>
            <v-icon> mdi-lock-reset </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>ປ່ຽນລະຫັດ</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-list>
        <v-list-item to="#" router exactc>
          <v-list-item-action>
            <v-icon> mdi-logout </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>ອອກຈາກລະບົບ</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar class="mywhite" :clipped-left="clipped" fixed app>
      <!-- button menu -->
      <v-app-bar-nav-icon class="grey--text" @click.stop="drawer = !drawer" />

      <!-- <v-toolbar-title v-text="title" /> -->
      <v-img src="/assetlogo.png" max-height="120" max-width="220"></v-img>
      <v-spacer />
      <v-btn icon to="#">
        <v-icon class="grey--text">mdi-account</v-icon>
      </v-btn>
      <v-toolbar-title class="grey--text" v-text="username" />
      <v-btn icon to="#">
        <v-icon class="grey--text">mdi-logout</v-icon>
      </v-btn>
    </v-app-bar>
    <!-- main space -->
    <v-main class="mywhite">
      <v-container fluid>
        <Nuxt />
      </v-container>
    </v-main>
    <!-- footer -->
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
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
          icon: '',
          title: 'ຂໍ້ມູນຜູ້ໃຊ້',
          to: '/inspire',
        },
        {
          icon: '',
          title: 'ຂໍ້ມູນພະນັກງານ',
          to: '/inspire',
        },
        {
          icon: '',
          title: 'ຂໍ້ມູນພະແນກ',
          to: '/inspire',
        },
        {
          icon: '',
          title: 'ຂໍ້ມູນຊັບສິນ',
          to: '/inspire',
        },
      ],

      itemrps: [
        {
          icon: '',
          title: 'ຊັບສິນທັງຫມົດ',
          to: '/test',
        },
        {
          icon: '',
          title: 'ອຸປະກອນຕ່າງໆ',
          to: '/inspire',
        },
        {
          icon: '',
          title: 'ຫ້ອງ',
          to: '/inspire',
        },
        {
          icon: '',
          title: 'ອາຄານ',
          to: '/inspire',
        },
        {
          icon: '',
          title: 'ຜູ້ສະຫນອງ',
          to: '/inspire',
        },
      ],

      miniVariant: false,
      title: 'Asset System',
      username: 'Nilammone',
    }
  },
}
</script>
