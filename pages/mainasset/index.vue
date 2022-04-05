<template>
  <div class="mt-3 container">
    <h3 class="mytxt">
      <v-icon>mdi-clipboard-text-multiple</v-icon> ຂໍ້ມູນຊັບສິນ
    </h3>
    <hr />

    <v-data-table
      :headers="headers"
      :items="desserts"
      :search="search"
      sort-by="usernam"
      class="elevation-1 mt-5"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Asset</v-toolbar-title>
          <v-spacer></v-spacer>

          <!-- s search -->
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="ຄົ້ນຫາ"
            single-line
            hide-details
            class="mr-5"
          ></v-text-field>
          <!-- e search -->

          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                color="success"
                dark
                class="mb-2"
                fab
                v-bind="attrs"
                v-on="on"
              >
                <v-icon>mdi-plus</v-icon>
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.asset_no"
                        label="ລະຫັດຊັບສິນ
              "
                        :rules="noRules"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.asset_name"
                        label="ຊື່ຊັບສິນ"
                        :rules="nameRules"
                      ></v-text-field>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.asset_group_id"
                        :items="itemsgroup"
                        item-value="gass_id"
                        item-text="gass_name"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="ຫມວດຊັບສິນ"
                        required
                      ></v-select>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.asset_group_id"
                        :items="itemsgroup"
                        item-value="gass_id"
                        item-text="gass_name"
                        @change="getidgroupasset"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="ຫມວດຊັບສິນ"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.asset_type_id"
                        :items="itemstype"
                        item-value="tass_id"
                        item-text="tass_name"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="ປະເພດຊັບສິນ"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.asset_startdate"
                        label="ວັນທີປ້ອນຂໍ້ມູນຊັບສິນ"
                        type="date"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.asset_enddate"
                        label="ວັນທີຫມົດອາຍຸຂອງຊັບສິນ"
                        type="date"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.asset_sp_id"
                        :items="itemssuppiler"
                        item-value="sp_id"
                        item-text="sp_name"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="ຜູ້ສະຫນອງ"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.asset_room_id"
                        :items="itemsroom"
                        item-value="room_id"
                        item-text="room_no"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="ຫ້ອງ"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.asset_building_id"
                        :items="itemsbuild"
                        item-value="bd_id"
                        item-text="bd_no"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="ຕຶກ"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.asset_status"
                        :items="itemsstatus"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="ສະຖານະ"
                        required
                      ></v-select>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  ຍົກເລີກ
                </v-btn>
                <v-btn color="blue darken-1" text @click="save"> ບັນທຶກ </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title>ທ່ານຕ້ອງການລົບ ແທ້ ຫລື ບໍ?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="deleteItemConfirm(editedItem.asset_id)"
                  >ຕົກລົງ</v-btn
                >
                <v-btn color="blue darken-1" text @click="closeDelete"
                  >ຍົກເລີກ</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template #[`item.numlist`]="{ item }">
        {{ desserts.indexOf(item) + 1 }}
      </template>
      <template #[`item.actions`]="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
    <!-- s alert -->
    <SuccessAlert :snackbar="snackbar"></SuccessAlert>
    <!-- e alert -->
  </div>
</template>

<script>
import SuccessAlert from '@/components/SuccessAlert'

export default {
  components: {
    SuccessAlert,
  },
  data() {
    return {
      search: '',
      snackbar: false,
      carouselInterval: null,
      text: 'Success!.',
      dialog: false,
      dialogDelete: false,
      noRules: [
        (v) => !!v || 'Asset_no is required',
        (v) => v.length <= 20 || 'Asset_no must be less than 20 characters',
      ],
      nameRules: [
        (v) => !!v || 'Asset_name is required',
        (v) => v.length <= 100 || 'Asset_name must be less than 100 characters',
      ],
      itemsgroup: [],
      itemstype: [],
      itemssuppiler: [],
      itemsroom: [],
      itemsbuild: [],
      itemsstatus: ['Active', 'In active'],
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
          text: 'ສະຖານະ',
          value: 'asset_status',
          align: 'center',
        },
        {
          text: 'ວັນທີເພີ່ມຊັບສິນ',
          value: 'asset_create_at',
          align: 'center',
        },
        {
          text: 'ວັນທີແກ້ໄຂຊັບສິນ',
          value: 'asset_move_at',
          align: 'center',
        },
        {
          text: 'ວັນທີລ້າງຊັບສິນ',
          value: 'asset_clearing_at',
          align: 'center',
        },

        { text: 'ຈັດການ', value: 'actions', sortable: false, align: 'center' },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        asset_no: '',
        asset_name: '',
        asset_group_id: '',
        asset_type_id: '',
        asset_startdate: '',
        asset_enddate: '',
        asset_sp_id: '',
        asset_room_id: '',
        asset_building_id: '',
        asset_user_id: '',
        asset_status: '',
      },
      defaultItem: {
        asset_no: '',
        asset_name: '',
        asset_group_id: '',
        asset_type_id: '',
        asset_startdate: '',
        asset_enddate: '',
        asset_sp_id: '',
        asset_room_id: '',
        asset_building_id: '',
        asset_user_id: '',
        asset_status: '',
      },
    }
  },

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'Add asset' : 'ແກ້ໄຂ ຂໍ້ມູນຊັບສິນ'
    },
  },

  watch: {
    dialog(val) {
      val || this.close()
    },
    dialogDelete(val) {
      val || this.closeDelete()
    },
  },

  created() {
    this.initialize()
    this.getDataGroupassets()
    // this.getDataTypeassets()
    this.getDataSuppilers()
    this.getDataRooms()
    this.getDataBuildings()
  },

  methods: {
    async initialize() {
      try {
        await this.$axios.get('/getdataJoinmore').then((res) => {
          this.desserts = res.data
        })
      } catch (err) {
        console.log(err)
      }
    },

    async getDataGroupassets() {
      try {
        await this.$axios.get('/getGroupassetOnlyActive').then((res) => {
          this.itemsgroup = res.data
        })
      } catch (err) {
        console.log(err)
      }
    },

    // async getDataTypeassets() {
    //   try {
    //     await this.$axios.get('/getTypeassetOnlyActive').then((res) => {
    //       this.itemstype = res.data
    //     })
    //   } catch (err) {
    //     console.log(err)
    //   }
    // },

    // check and get group asset id
    async getidgroupasset(gass_id) {
      try {
        await this.$axios.get(`getByGroupassetId/${gass_id}`).then((res) => {
          this.itemstype = res.data
        })
      } catch (err) {
        console.log(err)
      }
    },

    async getDataSuppilers() {
      try {
        await this.$axios.get('/suppilers').then((res) => {
          this.itemssuppiler = res.data
        })
      } catch (err) {
        console.log(err)
      }
    },

    async getDataRooms() {
      try {
        await this.$axios.get('/getRoomOnlyActive').then((res) => {
          this.itemsroom = res.data
        })
      } catch (err) {
        console.log(err)
      }
    },

    async getDataBuildings() {
      try {
        await this.$axios.get('/getBuildingOnlyActive').then((res) => {
          this.itemsbuild = res.data
        })
      } catch (err) {
        console.log(err)
      }
    },

    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    async deleteItemConfirm(assetId) {
      try {
        await this.$axios.delete(`/assets/${assetId}`).then((res) => {
          console.log('Delete completed!')
        })
        this.initialize()
        this.dialogDelete = false
      } catch (err) {
        console.log(err)
      }
    },

    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete() {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    async save() {
      if (this.editedIndex > -1) {
        const sendresdata = {
          asset_no: this.editedItem.asset_no,
          asset_name: this.editedItem.asset_name,
          asset_group_id: this.editedItem.asset_group_id,
          asset_type_id: this.editedItem.asset_type_id,
          asset_startdate: this.editedItem.asset_startdate,
          asset_enddate: this.editedItem.asset_enddate,
          asset_sp_id: this.editedItem.asset_sp_id,
          asset_room_id: this.editedItem.asset_room_id,
          asset_building_id: this.editedItem.asset_building_id,
          asset_user_id: this.$auth.user.id,
          asset_status: this.editedItem.asset_status,
        }

        try {
          await this.$axios
            .put(`/assets/${this.editedItem.asset_id}`, sendresdata)
            .then((res) => {
              console.log('edit completed!')
            })

          this.initialize()
        } catch (err) {
          console.log(err)
        }
      } else {
        const getresdata = {
          asset_no: this.editedItem.asset_no,
          asset_name: this.editedItem.asset_name,
          asset_group_id: this.editedItem.asset_group_id,
          asset_type_id: this.editedItem.asset_type_id,
          asset_startdate: this.editedItem.asset_startdate,
          asset_enddate: this.editedItem.asset_enddate,
          asset_sp_id: this.editedItem.asset_sp_id,
          asset_room_id: this.editedItem.asset_room_id,
          asset_building_id: this.editedItem.asset_building_id,
          asset_user_id: this.$auth.user.id,
          asset_status: this.editedItem.asset_status,
        }

        try {
          await this.$axios.post('/assets', getresdata).then((res) => {
            console.log('Insert completed!')
          })

          this.carouselInterval = setInterval(() => {
            this.snackbar = true
          }, 800)

          this.initialize()

          this.snackbar = false
        } catch (err) {
          console.log(err)
        }
      }
      this.close()
    },
  },
}
</script>
