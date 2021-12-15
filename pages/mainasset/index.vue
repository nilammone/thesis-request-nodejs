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
            label="Search"
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
                        v-model="editedItem.assetno"
                        label="Asset_no
              "
                        :rules="noRules"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.assetname"
                        label="Asset_name"
                        :rules="nameRules"
                      ></v-text-field>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.assetgroupid"
                        label="Asset_groupid"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.assetgroupid"
                        :items="itemsgroup"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="Asset_group"
                        required
                      ></v-select>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.assettypeid"
                        label="Asset_typeid"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.assettypeid"
                        :items="itemstype"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="Asset_type"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.assetstartdate"
                        label="Asset_start_date"
                        type="date"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.assetenddate"
                        label="Asset_end_date"
                        type="date"
                      ></v-text-field>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.assetsuppilerid"
                        label="Asset_suppilerid"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.assetsuppilerid"
                        :items="itemssuppiler"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="Asset_suppiler"
                        required
                      ></v-select>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.assetroomid"
                        label="Asset_roomid"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.assetroomid"
                        :items="itemsroom"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="Asset_room"
                        required
                      ></v-select>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.assetbuildingid"
                        label="Asset_buildingid"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.assetbuildingid"
                        :items="itemsbuild"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="Asset_building"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.assetuserid"
                        label="Asset_user"
                      ></v-text-field>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.assetstatus"
                        label="Asset_status"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.assetstatus"
                        :items="itemsstatus"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="Asset_status"
                        required
                      ></v-select>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  Cancel
                </v-btn>
                <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title>ທ່ານຕ້ອງການລົບ ແທ້ ຫລື ບໍ?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="deleteItemConfirm"
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
        {{
          desserts
            .map(function (x) {
              return x.id
            })
            .indexOf(item.id) + 1
        }}
      </template>
      <template #[`item.actions`]="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      search: '',
      dialog: false,
      dialogDelete: false,
      noRules: [
        (v) => !!v || 'Asset_no is required',
        (v) => v.length <= 20 || 'Asset_no must be less than 20 characters',
      ],
      nameRules: [
        (v) => !!v || 'Asset_name is required',
        (v) => v.length <= 30 || 'Asset_name must be less than 30 characters',
      ],
      itemsgroup: ['Table', 'Vehicles', 'Chair', 'Cabinet', 'Computer'],
      itemstype: ['Table01', 'Table02'],
      itemssuppiler: ['Suppiler01', 'Suppiler02'],
      itemsroom: ['Class01', 'Class02'],
      itemsbuild: ['AC01', 'AC02'],
      itemsstatus: ['Active', 'In active'],
      headers: [
        {
          text: 'No',
          align: 'center',
          sortable: false,
          value: 'numlist',
        },
        { text: 'Asset_id', value: 'assetid', align: 'center' },
        {
          text: 'Asset_no',
          value: 'assetno',
          align: 'center',
        },
        {
          text: 'Asset_name',
          value: 'assetname',
          align: 'center',
        },
        {
          text: 'Asset_group',
          value: 'assetgroupid',
          align: 'center',
        },
        {
          text: 'Asset_type',
          value: 'assettypeid',
          align: 'center',
        },
        {
          text: 'Asset_start_date',
          value: 'assetstartdate',
          align: 'center',
        },
        {
          text: 'Asset_end_date',
          value: 'assetenddate',
          align: 'center',
        },
        {
          text: 'Asset_suppiler',
          value: 'assetsuppilerid',
          align: 'center',
        },
        {
          text: 'Asset_room',
          value: 'assetroomid',
          align: 'center',
        },
        {
          text: 'Asset_building',
          value: 'assetbuildingid',
          align: 'center',
        },
        {
          text: 'Asset_user',
          value: 'assetuserid',
          align: 'center',
        },
        {
          text: 'Asset_status',
          value: 'assetstatus',
          align: 'center',
        },
        {
          text: 'Asset_create_at',
          value: 'assetcreateat',
          align: 'center',
        },
        {
          text: 'Asset_move_at',
          value: 'assetmoveat',
          align: 'center',
        },
        {
          text: 'Asset_clearing_at',
          value: 'assetclearingat',
          align: 'center',
        },

        { text: 'Actions', value: 'actions', sortable: false, align: 'center' },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        assetid: 0,
        assetno: '',
        assetname: '',
        assetgroupid: '',
        assettypeid: '',
        assetstartdate: '',
        assetenddate: '',
        assetsuppilerid: '',
        assetroomid: '',
        assetbuildingid: '',
        assetuserid: '',
        assetstatus: '',
        assetcreateat: '',
        assetmoveat: '',
        assetclearingat: '',
      },
      defaultItem: {
        assetid: 0,
        assetno: '',
        assetname: '',
        assetgroupid: '',
        assettypeid: '',
        assetstartdate: '',
        assetenddate: '',
        assetsuppilerid: '',
        assetroomid: '',
        assetbuildingid: '',
        assetuserid: '',
        assetstatus: '',
        assetcreateat: '',
        assetmoveat: '',
        assetclearingat: '',
      },
    }
  },

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'Add Asset' : 'Edit Asset'
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
  },

  methods: {
    initialize() {
      this.desserts = [
        {
          id: 10,
          assetid: 1,
          assetno: 'AS01',
          assetname: 'asset01',
          assetgroupid: 1,
          assettypeid: 2,
          assetstartdate: '2021-01-01',
          assetenddate: '2021-01-31',
          assetsuppilerid: 2,
          assetroomid: 2,
          assetbuildingid: 2,
          assetuserid: 'user01',
          assetstatus: 'Y',
          assetcreateat: '2021-01-01 10:00:00',
          assetmoveat: '',
          assetclearingat: '',
        },
        {
          id: 20,
          assetid: 2,
          assetno: 'AS02',
          assetname: 'asset02',
          assetgroupid: 1,
          assettypeid: 2,
          assetstartdate: '2021-01-01',
          assetenddate: '2021-01-31',
          assetsuppilerid: 2,
          assetroomid: 2,
          assetbuildingid: 2,
          assetuserid: 'user01',
          assetstatus: 'Y',
          assetcreateat: '2021-01-01 10:00:00',
          assetmoveat: '',
          assetclearingat: '',
        },
        {
          id: 30,
          assetid: 3,
          assetno: 'AS03',
          assetname: 'asset03',
          assetgroupid: 1,
          assettypeid: 2,
          assetstartdate: '2021-01-01',
          assetenddate: '2021-01-31',
          assetsuppilerid: 2,
          assetroomid: 2,
          assetbuildingid: 2,
          assetuserid: 'user01',
          assetstatus: 'Y',
          assetcreateat: '2021-01-01 10:00:00',
          assetmoveat: '',
          assetclearingat: '',
        },
        {
          id: 40,
          assetid: 4,
          assetno: 'AS04',
          assetname: 'asset04',
          assetgroupid: 1,
          assettypeid: 2,
          assetstartdate: '2021-01-01',
          assetenddate: '2021-01-31',
          assetsuppilerid: 2,
          assetroomid: 2,
          assetbuildingid: 2,
          assetuserid: 'user01',
          assetstatus: 'Y',
          assetcreateat: '2021-01-01 10:00:00',
          assetmoveat: '',
          assetclearingat: '',
        },
        {
          id: 50,
          assetid: 5,
          assetno: 'AS05',
          assetname: 'asset05',
          assetgroupid: 1,
          assettypeid: 2,
          assetstartdate: '2021-01-01',
          assetenddate: '2021-01-31',
          assetsuppilerid: 2,
          assetroomid: 2,
          assetbuildingid: 2,
          assetuserid: 'user01',
          assetstatus: 'Y',
          assetcreateat: '2021-01-01 10:00:00',
          assetmoveat: '',
          assetclearingat: '',
        },
      ]
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

    deleteItemConfirm() {
      this.desserts.splice(this.editedIndex, 1)
      this.closeDelete()
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

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
        // console.log(this.desserts[0].id)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>
