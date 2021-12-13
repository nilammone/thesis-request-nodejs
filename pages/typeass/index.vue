<template>
  <div class="mt-3 container">
    <h3 class="mytxt">ຂໍ້ມູນປະເພດຊັບສິນ</h3>
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
          <v-toolbar-title>Type Asset</v-toolbar-title>
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
                        v-model="editedItem.typeassetid"
                        label="Type_asset_id
              "
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.typeassetname"
                        label="Type_asset_name"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.typeassetgroupid"
                        label="Type_asset_groupid"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.typeassetstatus"
                        label="Type_asset_status"
                      ></v-text-field>
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
      headers: [
        {
          text: 'No',
          align: 'center',
          sortable: false,
          value: 'numlist',
        },
        { text: 'Type_asset_id', value: 'typeassetid', align: 'center' },
        { text: 'Type_asset_name', value: 'typeassetname', align: 'center' },
        {
          text: 'Type_asset_groupid',
          value: 'typeassetgroupid',
          align: 'center',
        },
        {
          text: 'Type_asset_status',
          value: 'typeassetstatus',
          align: 'center',
        },
        { text: 'Actions', value: 'actions', sortable: false, align: 'center' },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        typeassetid: 0,
        typeassetname: '',
        typeassetgroupid: 0,
        typeassetstatus: '',
      },
      defaultItem: {
        typeassetid: 0,
        typeassetname: '',
        typeassetgroupid: 0,
        typeassetstatus: '',
      },
    }
  },

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'Add new' : 'Edit'
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
          typeassetid: 1,
          typeassetname: 'Table01',
          typeassetgroupid: 100,
          typeassetstatus: 'Y',
        },
        {
          id: 20,
          typeassetid: 2,
          typeassetname: 'Table02',
          typeassetgroupid: 100,
          typeassetstatus: 'Y',
        },
        {
          id: 30,
          typeassetid: 3,
          typeassetname: 'Cheep',
          typeassetgroupid: 200,
          typeassetstatus: 'Y',
        },
        {
          id: 40,
          typeassetid: 4,
          typeassetname: 'Van',
          typeassetgroupid: 200,
          typeassetstatus: 'Y',
        },
        {
          id: 50,
          typeassetid: 5,
          typeassetname: 'laptop',
          typeassetgroupid: 500,
          typeassetstatus: 'Y',
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
