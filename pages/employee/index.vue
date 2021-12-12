<template>
  <div class="mt-3 container">
    <h3 class="mytxt">ຂໍ້ມູນພະນັກງານ</h3>
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
          <v-toolbar-title>Employee</v-toolbar-title>
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
                <v-icon>mdi-account-plus</v-icon>
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
                        v-model="editedItem.firstname"
                        label="firstname
              "
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.lastname"
                        label="lastname"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.deptid"
                        label="deptid"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.contact"
                        label="contact"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.address"
                        label="address"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.image"
                        label="image"
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
        { text: 'Firstname', value: 'firstname', align: 'center' },
        { text: 'Lastname', value: 'lastname', align: 'center' },
        { text: 'Dept_id', value: 'deptid', align: 'center' },
        { text: 'Contact', value: 'contact', align: 'center' },
        { text: 'Address', value: 'address', align: 'center' },
        { text: 'Image', value: 'image', align: 'center' },
        { text: 'Actions', value: 'actions', sortable: false, align: 'center' },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        firstname: '',
        lastname: '',
        deptid: 0,
        contact: '',
        address: '',
        image: '',
      },
      defaultItem: {
        firstname: '',
        lastname: '',
        deptid: 0,
        contact: '',
        address: '',
        image: '',
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
          firstname: 'Chantha',
          lastname: 'mono',
          deptid: 1,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/01.png',
        },
        {
          id: 20,
          firstname: 'Chantha2',
          lastname: 'mono2',
          deptid: 12,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/012.png',
        },
        {
          id: 30,
          firstname: 'Chantha3',
          lastname: 'mono3',
          deptid: 13,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/013.png',
        },
        {
          id: 40,
          firstname: 'Chantha4',
          lastname: 'mono4',
          deptid: 14,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/014.png',
        },
        {
          id: 50,
          firstname: 'Chantha5',
          lastname: 'mono5',
          deptid: 15,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/015.png',
        },
        {
          id: 60,
          firstname: 'Chantha7',
          lastname: 'mono7',
          deptid: 17,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/017.png',
        },
        {
          id: 70,
          firstname: 'Chantha8',
          lastname: 'mono8',
          deptid: 18,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/018.png',
        },
        {
          id: 80,
          firstname: 'Chantha9',
          lastname: 'mono9',
          deptid: 19,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/019.png',
        },
        {
          id: 90,
          firstname: 'Chantha10',
          lastname: 'mono10',
          deptid: 110,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/0110.png',
        },
        {
          id: 95,
          firstname: 'Chantha11',
          lastname: 'mono11',
          deptid: 111,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/0111.png',
        },
        {
          id: 97,
          firstname: 'Chantha12',
          lastname: 'mono12',
          deptid: 112,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/0112.png',
        },
        {
          id: 100,
          firstname: 'Chantha13',
          lastname: 'mono13',
          deptid: 113,
          contact: '020 55556666',
          address: 'Vientiane',
          image: '/0113.png',
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
