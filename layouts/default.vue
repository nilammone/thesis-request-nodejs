<template>
  <v-app>
    <v-app-bar :clipped-left="clipped" fixed app>
      <span> Compare Node Js Vs Spring Boot Java </span>

      <v-spacer />
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
        // {
        //   icon: '',
        //   title: 'ຂໍ້ມູນຜູ້ໃຊ້',
        //   to: '/user',
        // },
        {
          icon: '',
          // title: 'ຂໍ້ມູນພະນັກງານ',
          title: 'ຂໍ້ມູນຜູ້ໃຊ້',
          to: '/employee',
        },
        {
          icon: '',
          title: 'ຂໍ້ມູນພາກວິຊາ/ພະແນກ',
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
  methods: {
    async logout() {
      try {
        await this.$auth.logout()

        await this.$router.push('/login')
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>

<style scoped>
.myresizebt {
  width: 20px;
}
</style>
