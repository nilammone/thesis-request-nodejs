<template>
  <div class="mt-3 container">
    <h3 class="mytxt"><v-icon>mdi-file-multiple</v-icon> ລາຍງານຍ່ອຍ</h3>
    <hr class="primary" />
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
                <v-btn color="info" @click="searchData()">ຄົ້ນຫາ</v-btn>
              </v-col>
            </v-row>
          </v-form>
        </v-card>

        <!-- s table -->
        <v-card class="mt-6">
          <v-card-title>
            <!-- Report details -->
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
      </v-col>
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
        {
          text: 'ລະຫັດຊັບສິນ',
          value: 'asset_no',
          align: 'center',
        },
        {
          text: 'ຊື່ຊັບສິນ',
          value: 'asset_name',
          align: 'center',
        },
        {
          text: 'ຫມວດຊັບສິນ',
          value: 'gass_name',
          align: 'center',
        },
        {
          text: 'ປະເພດຊັບສິນ',
          value: 'tass_name',
          align: 'center',
        },
        {
          text: 'ມື້ປ້ອນຂໍ້ມູນ',
          value: 'asset_startdate',
          align: 'center',
        },
        {
          text: 'ມື້ຫມົດອາຍຸຂອງຊັບສິນ',
          value: 'asset_enddate',
          align: 'center',
        },
        {
          text: 'ຜູ້ສະຫນອງ',
          value: 'sp_name',
          align: 'center',
        },
        {
          text: 'ຜູ້ສະຫນັບສະຫນູນ',
          value: 'sps_name',
          align: 'center',
        },
        {
          text: 'ຫ້ອງ',
          value: 'room_no',
          align: 'center',
        },
        {
          text: 'ຕຶກ',
          value: 'bd_no',
          align: 'center',
        },
        {
          text: 'ພາກສ່ວນຮັບຜິດຊອບ',
          value: 'v1deptname',
          align: 'center',
        },
        {
          text: 'ຫມາຍເຫດ',
          value: 'asset_remark',
          align: 'center',
        },
        // {
        //   text: 'ສະຖານະ',
        //   value: 'asset_status',
        //   align: 'center',
        // },
        // {
        //   text: 'ວັນທີເພີ່ມຊັບສິນ',
        //   value: 'asset_create_at',
        //   align: 'center',
        // },
        // {
        //   text: 'ວັນທີແກ້ໄຂຊັບສິນ',
        //   value: 'asset_move_at',
        //   align: 'center',
        // },
        // {
        //   text: 'ວັນທີລ້າງຊັບສິນ',
        //   value: 'asset_clearing_at',
        //   align: 'center',
        // },

        // { text: 'ຈັດການ', value: 'actions', sortable: false, align: 'center' },
      ],
      desserts: [],
    }
  },

  created() {
    this.initialize()
  },

  methods: {
    async initialize() {
      try {
        await this.$axios.get('/getdataJoinmoreActiveNotDate').then((res) => {
          this.desserts = res.data
        })
      } catch (err) {
        console.log(err)
      }
    },

    async searchData() {
      try {
        await this.$axios
          .get(`/getdataJoinmoreActiveByDate/${this.sdate}/${this.edate}`)
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
