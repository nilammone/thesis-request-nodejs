<template>
  <div class="mt-3 container">
    <h3 class="mytxt"><v-icon>mdi-city-variant</v-icon> ຂໍ້ມູນອາຄານ</h3>
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
          <v-toolbar-title>Building</v-toolbar-title>
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
                        v-model="editedItem.buildingid"
                        label="Building_id
              "
                        :rules="[(v) => !!v || 'Item is required']"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.buildingno"
                        label="Building_no"
                        :rules="nameRules"
                      ></v-text-field>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.buildingtypeid"
                        label="Building_typeid"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.buildingtypeid"
                        :items="itemstype"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="Building_typeid"
                        required
                      ></v-select>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.buildingstatus"
                        label="Building_status"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.buildingstatus"
                        :items="itemsstatus"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="Building_status"
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
      nameRules: [
        (v) => !!v || 'Building_no is required',
        (v) => v.length <= 10 || 'Building_no must be less than 10 characters',
      ],
      itemstype: ['build01', 'build02', 'build03', 'build04'],
      itemsstatus: ['Active', 'In active'],
      headers: [
        {
          text: 'No',
          align: 'center',
          sortable: false,
          value: 'numlist',
        },
        { text: 'Building_id', value: 'buildingid', align: 'center' },
        {
          text: 'Building_no',
          value: 'buildingno',
          align: 'center',
        },
        {
          text: 'Building_typeid',
          value: 'buildingtypeid',
          align: 'center',
        },
        {
          text: 'Building_status',
          value: 'buildingstatus',
          align: 'center',
        },
        { text: 'Actions', value: 'actions', sortable: false, align: 'center' },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        buildingid: 0,
        buildingno: '',
        buildingtypeid: '',
        buildingstatus: '',
      },
      defaultItem: {
        buildingid: 0,
        buildingno: '',
        buildingtypeid: '',
        buildingstatus: '',
      },
    }
  },

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'Add Building' : 'Edit Building'
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
          buildingid: 1,
          buildingno: 'AC01',
          buildingtypeid: 1,
          buildingstatus: 'Active',
        },
        {
          id: 20,
          buildingid: 2,
          buildingno: 'AC02',
          buildingtypeid: 1,
          buildingstatus: 'Active',
        },
        {
          id: 30,
          buildingid: 3,
          buildingno: 'AC03',
          buildingtypeid: 2,
          buildingstatus: 'Active',
        },
        {
          id: 40,
          buildingid: 4,
          buildingno: 'AC04',
          buildingtypeid: 2,
          buildingstatus: 'Active',
        },
        {
          id: 50,
          buildingid: 5,
          buildingno: 'AC05',
          buildingtypeid: 3,
          buildingstatus: 'Active',
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
