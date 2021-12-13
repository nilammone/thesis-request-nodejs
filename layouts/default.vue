<template>
  <v-app>
    <!-- menu left size -->
    <v-navigation-drawer
      color="#575757"
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item to="/" router exactc>
          <v-list-item-action>
            <v-icon color="white"> mdi-home </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title style="color: white">ຫນ້າຫລັກ</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-list>
        <v-list-item router exactc>
          <v-list-item-action>
            <v-icon color="white"> mdi-cog-outline </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title style="color: white"
              >ບໍລິຫານຈັດການ</v-list-item-title
            >
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
            <v-icon color="white">{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title
              style="color: white; font-size: 0.9em"
              v-text="item.title"
            />
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-list-group prepend-icon="icon-icon_fee" no-action>
        <template v-slot:activator>
          <v-list-item-content>
            <v-list-item-title style="color: white">
              ຂໍ້ມູນຊັບສິນ
            </v-list-item-title>
          </v-list-item-content>
        </template>
        <v-list-item to="/1">
          <v-list-item-title style="color: white; font-size: 0.8em">
            ຊັບສິນ
          </v-list-item-title>
        </v-list-item>
        <v-list-item to="/groupass">
          <v-list-item-title style="color: white; font-size: 0.8em">
            ຫມວດຊັບສິນ
          </v-list-item-title>
        </v-list-item>
        <v-list-item to="/typeass">
          <v-list-item-title style="color: white; font-size: 0.8em">
            ປະເພດຊັບສິນ
          </v-list-item-title>
        </v-list-item>
        <v-list-item to="/building">
          <v-list-item-title style="color: white; font-size: 0.8em">
            ອາຄານ
          </v-list-item-title>
        </v-list-item>
        <v-list-item to="/buildingtype">
          <v-list-item-title style="color: white; font-size: 0.8em">
            ປະເພດອາຄານ
          </v-list-item-title>
        </v-list-item>
        <v-list-item to="/6">
          <v-list-item-title style="color: white; font-size: 0.8em">
            ຫ້ອງ
          </v-list-item-title>
        </v-list-item>
        <v-list-item to="/7">
          <v-list-item-title style="color: white; font-size: 0.8em">
            ປະເພດຫ້ອງ
          </v-list-item-title>
        </v-list-item>
      </v-list-group>

      <v-list>
        <v-list-item to="/8" router exactc>
          <v-list-item-action>
            <v-icon color="white"></v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title style="color: white"
              >ຂໍ້ມູນຜູ້ສະຫນອງ</v-list-item-title
            >
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-list>
        <v-list-item router exactc>
          <v-list-item-action>
            <v-icon color="white"> mdi-chart-areaspline </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title style="color: white">ລາຍງານ</v-list-item-title>
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
            <v-icon color="white">{{ itemrp.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title
              v-text="itemrp.title"
              style="color: white; font-size: 0.9em"
            />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-list>
        <v-list-item to="/changepassw" router exactc>
          <v-list-item-action>
            <v-icon color="white"> mdi-lock-reset </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title style="color: white"
              >ປ່ຽນລະຫັດ</v-list-item-title
            >
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-list>
        <v-list-item to="/logout" router exactc>
          <v-list-item-action>
            <v-icon color="white"> mdi-logout </v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title style="color: white"
              >ອອກຈາກລະບົບ</v-list-item-title
            >
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <!-- button menu -->
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <nuxt-link to="/">
        <v-img src="/assetlogo.png" max-height="120" max-width="220"></v-img>
      </nuxt-link>
      <v-spacer />
      <v-text-field
        class="mt-7 d-none d-md-flex shrink"
        outlined
        dense
        label="ຄົ້ນຫາ"
        prepend-inner-icon="mdi-magnify"
        rounded
      ></v-text-field>
      <v-btn icon to="#">
        <v-icon>mdi-account</v-icon>
      </v-btn>
      <v-toolbar-title v-text="username" />
      <v-btn icon to="#">
        <v-icon>mdi-logout</v-icon>
      </v-btn>
    </v-app-bar>
    <!-- main space -->
    <v-main>
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
          to: '/user',
        },
        {
          icon: '',
          title: 'ຂໍ້ມູນພະນັກງານ',
          to: '/employee',
        },
        {
          icon: '',
          title: 'ຂໍ້ມູນພະແນກ',
          to: '/department',
        },
      ],

      itemrps: [
        {
          icon: '',
          title: 'ລາຍງານລວມ',
          to: '/reportAll',
        },
        {
          icon: '',
          title: 'ລາຍງານຍ່ອຍ',
          to: '/reportSub',
        },
      ],

      miniVariant: false,
      title: 'Asset System',
      username: 'Nilammone',
    }
  },
}
</script>

<style scoped>
.myresizebt {
  width: 20px;
}
</style>
