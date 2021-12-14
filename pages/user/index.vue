<template>
  <div class="mt-3 container">
    <h3 class="mytxt">ຂໍ້ມູນຜູ້ໃຊ້</h3>
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
          <v-toolbar-title>User</v-toolbar-title>
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
                        v-model="editedItem.username"
                        label="username
              "
                        :rules="nameRules"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.email"
                        label="email"
                        :rules="emailRules"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.password"
                        type="password"
                        label="password"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.employeeid"
                        label="employeeid"
                      ></v-text-field>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.status"
                        label="status"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.status"
                        :items="itemsstatus"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="status"
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
        (v) => !!v || 'Username is required',
        (v) => v.length <= 30 || 'Username must be less than 30 characters',
      ],
      emailRules: [
        (v) => !!v || 'E-mail is required',
        (v) => /.+@.+/.test(v) || 'E-mail must be valid',
      ],
      itemsstatus: ['Admin', 'Auth', 'Inputer'],
      headers: [
        {
          text: 'No',
          align: 'center',
          sortable: false,
          value: 'numlist',
        },
        { text: 'Username', value: 'username', align: 'center' },
        { text: 'Email', value: 'email', align: 'center' },
        { text: 'Password', value: 'password', align: 'center' },
        { text: 'Employee_id', value: 'employeeid', align: 'center' },
        { text: 'Status', value: 'status', align: 'center' },
        { text: 'Actions', value: 'actions', sortable: false, align: 'center' },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        username: '',
        email: '',
        password: '',
        employeeid: 0,
        status: '',
      },
      defaultItem: {
        username: '',
        email: '',
        password: '',
        employeeid: 0,
        status: '',
      },
    }
  },

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'Add User' : 'Edit User'
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
          username: 'user01',
          email: 'user01@example.com',
          password: '-',
          employeeid: 100,
          status: 'isAdmin',
        },
        {
          id: 20,
          username: 'user02',
          email: 'user02@example.com',
          password: '-',
          employeeid: 200,
          status: 'inputer',
        },
        {
          id: 30,
          username: 'user03',
          email: 'user03@example.com',
          password: '-',
          employeeid: 300,
          status: 'inputer',
        },
        {
          id: 40,
          username: 'user04',
          email: 'user04@example.com',
          password: '-',
          employeeid: 400,
          status: 'inputer',
        },
        {
          id: 50,
          username: 'user05',
          email: 'user05@example.com',
          password: '-',
          employeeid: 500,
          status: 'inputer',
        },
        {
          id: 60,
          username: 'user06',
          email: 'user06@example.com',
          password: '-',
          employeeid: 600,
          status: 'inputer',
        },
        {
          id: 70,
          username: 'user07',
          email: 'user07@example.com',
          password: '-',
          employeeid: 700,
          status: 'inputer',
        },
        {
          id: 80,
          username: 'user08',
          email: 'user08@example.com',
          password: '-',
          employeeid: 800,
          status: 'inputer',
        },
        {
          id: 90,
          username: 'user09',
          email: 'user09@example.com',
          password: '-',
          employeeid: 900,
          status: 'inputer',
        },
        {
          id: 95,
          username: 'user10',
          email: 'user10@example.com',
          password: '-',
          employeeid: 1100,
          status: 'inputer',
        },
        {
          id: 97,
          username: 'user11',
          email: 'user11@example.com',
          password: '-',
          employeeid: 1200,
          status: 'inputer',
        },
        {
          id: 100,
          username: 'user12',
          email: 'user12@example.com',
          password: '-',
          employeeid: 1300,
          status: 'inputer',
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
