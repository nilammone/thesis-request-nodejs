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
            <template #[`item.actions`]="{ item }">
              <v-btn color="info" @click="editItem(item)">ເບິ່ງລາຍລະອຽດ</v-btn>
            </template>
          </v-data-table>
        </v-card>
        <!-- e table -->

        <h4 class="mt-4">ຈຳນວນຊັບສິນທັງຫມົດ: {{ sumtotalamount }}</h4>
      </v-col>
    </v-row>

    <!-- s show details -->
    <v-dialog v-if="getdatadialog" v-model="dialog" max-width="1000px">
      <v-card>
        <v-container>
          <!-- <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">ລະຫັດຊັບສິນ</th>
                  <th class="text-left">ຊື່ຊັບສິນ</th>
                  <th class="text-left">ປະເພດຊັບສິນ</th>
                  <th class="text-left">ຜູ້ສະຫນອງ</th>
                  <th class="text-left">ຜູ້ສະຫນັບສະຫນູນ</th>
                  <th class="text-left">ຫ້ອງ</th>
                  <th class="text-left">ຕຶກ</th>
                </tr>
              </thead>
              <tbody v-for="getdai in getdatadialog" :key="getdai.asset_id">
                <tr>
                  <td>{{ getdai.asset_no }}</td>
                  <td>{{ getdai.asset_name }}</td>
                  <td>{{ getdai.tass_name }}</td>
                  <td>{{ getdai.sp_name }}</td>
                  <td>{{ getdai.sps_name }}</td>
                  <td>{{ getdai.room_no }}</td>
                  <td>{{ getdai.bd_no }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table> -->

          <!-- s add new -->
          <v-data-table
            dense
            :headers="headers2"
            :items="getdatadialog"
            item-key="name"
            class="elevation-1"
          ></v-data-table>
          <!-- e add new -->
        </v-container>
      </v-card>
    </v-dialog>
    <!-- e show details -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      sdate: '',
      edate: '',
      search: '',
      dialog: false,
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
        { text: 'ຈັດການ', value: 'actions', sortable: false, align: 'center' },
      ],

      headers2: [
        { text: 'ລະຫັດຊັບສິນ', value: 'asset_no' },
        { text: 'ຊື່ຊັບສິນ', value: 'asset_name' },
        { text: 'ປະເພດຊັບສິນ', value: 'tass_name' },
        { text: 'ຜູ້ສະຫນອງ', value: 'sp_name' },
        { text: 'ຜູ້ສະຫນັບສະຫນູນ', value: 'sps_name' },
        { text: 'ຫ້ອງ', value: 'room_no' },
        { text: 'ຕຶກ', value: 'bd_no' },
      ],
      desserts: [],
      getdatadialog: [],
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
        await this.$axios.get('/getdataReportAllNotDate').then((res) => {
          this.desserts = res.data
        })
      } catch (err) {
        console.log(err)
      }
    },

    async searchFromDate() {
      try {
        await this.$axios
          .get(`/getdataReportAll/${this.sdate}/${this.edate}`)
          .then((res) => {
            this.desserts = res.data
          })
      } catch (err) {
        console.log(err)
      }
    },

    async editItem(item) {
      await this.$axios
        .get(
          `/getdataJoinmoreActiveByUseridGroupid/${item.asset_user_id}/${item.asset_group_id}`
        )
        .then((res) => {
          this.getdatadialog = res.data
        })
        .catch((err) => {
          console.log(err)
        })

      this.dialog = true
    },
  },
}
</script>
