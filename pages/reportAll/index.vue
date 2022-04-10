<template>
  <div class="mt-3 container">
    <h3 class="mytxt"><v-icon>mdi-file</v-icon> ລາຍງານລວມ</h3>
    <hr class="success" />
    <v-row class="mt-6">
      <v-col cols="12" md="12">
        <v-card>
          <v-form>
            <v-row class="ml-6">
              <v-col cols="12" md="4">
                <v-text-field
                  v-model="sdate"
                  type="date"
                  label="ເລີ່ມວັນທີ"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="4">
                <v-text-field
                  v-model="edate"
                  type="date"
                  label="ເຖິງວັນທີ"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="4" class="mt-4">
                <v-btn color="success" @click="searchFromDate()">ຄົ້ນຫາ</v-btn>
              </v-col>
            </v-row>
          </v-form>
        </v-card>

        <!-- s table -->
        <v-card class="mt-6">
          <v-card-title>
            <!-- Report All -->
            <v-spacer></v-spacer>
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
          </v-card-title>
          <v-data-table :headers="headers" :items="desserts" :search="search">
            <template #[`item.numlist`]="{ item }">
              {{ desserts.indexOf(item) + 1 }}
            </template>
          </v-data-table>
        </v-card>
        <!-- e table -->

        <h4 class="mt-4">ຈຳນວນຊັບສິນທັງຫມົດ: {{ sumtotalamount }}</h4>
      </v-col>
      <!-- <v-col cols="12" md="3" class="ml-auto">
        <v-navigation-drawer permanent>
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title style="color: #1b8537">
                ລາຍງານລວມ
              </v-list-item-title>
              <v-list-item-subtitle> ເລືອກລາຍງານຂອງທ່ານ </v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-divider></v-divider>

          <v-list dense nav>
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title>ຊັບສິນທັງຫມົດ</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title>ອຸປະກອນ</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title>ອາຄານ</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title>ຫ້ອງ</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title>ຜູ້ສະຫນອງ</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-navigation-drawer>
      </v-col> -->
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sdate: '',
      edate: '',
      search: '',
      headers: [
        {
          text: 'ລຳດັບ',
          align: 'center',
          sortable: false,
          value: 'numlist',
        },
        { text: 'ພະແນກ', value: 'v1deptname' },
        { text: 'ຫມວດຊັບສິນ', value: 'gass_name' },
        { text: 'ຈຳນວນ', value: 'total_amount' },
      ],
      desserts: [],
    }
  },

  computed: {
    sumtotalamount() {
      var sumva = 0
      this.desserts.map((v) => {
        sumva += v.total_amount
      })
      return sumva
    },
  },

  created() {
    this.getDataReportAllNotDate()
  },
  methods: {
    async getDataReportAllNotDate() {
      try {
        this.$axios.get('/getdataReportAllNotDate').then((res) => {
          this.desserts = res.data
        })
      } catch (err) {
        console.log(err)
      }
    },

    async searchFromDate() {
      try {
        this.$axios
          .get(`/getdataReportAll/${this.sdate}/${this.edate}`)
          .then((res) => {
            this.desserts = res.data
          })
      } catch (err) {
        console.log(err)
      }
    },
  },
}
</script>
