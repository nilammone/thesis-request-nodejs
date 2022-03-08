<template>
  <div class="mt-3 container">
    <h3 class="mytxt"><v-icon>mdi-account</v-icon> ຂໍ້ມູນຜູ້ໃຊ້</h3>
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
            label="ຄົ້ນຫາ"
            single-line
            hide-details
            class="mr-5"
          ></v-text-field>
          <!-- e search -->

          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                disabled
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
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.password"
                        type="password"
                        label="password"
                      ></v-text-field>
                    </v-col> -->
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.user_emp_id"
                        label="employeeid"
                      ></v-text-field>
                    </v-col> -->
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.status"
                        label="status"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.isAdmin"
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
      <template #[`item.isAdmin`]="{ item }">
        {{ item.isAdmin === '1' ? 'Admin' : 'User' }}
      </template>
      <template #[`item.actions`]="{ item }">
        <v-icon disabled small class="mr-2" @click="editItem(item)">
          mdi-pencil
        </v-icon>
        <v-icon disabled small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import axios from 'axios'
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
          text: 'ລຳດັບ',
          align: 'center',
          sortable: false,
          value: 'numlist',
        },
        { text: 'ຊື່ຜູ້ໃຊ້', value: 'username', align: 'center' },
        { text: 'ອີເມວ', value: 'email', align: 'center' },
        // { text: 'Password', value: 'password', align: 'center' },
        // { text: 'Employee_id', value: 'user_emp_id', align: 'center' },
        { text: 'ສະຖານະ', value: 'isAdmin', align: 'center' },
        { text: 'ຈັດການ', value: 'actions', sortable: false, align: 'center' },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        username: '',
        email: '',
        // password: '',
        // employeeid: 0,
        status: '',
      },
      defaultItem: {
        username: '',
        email: '',
        // password: '',
        // employeeid: 0,
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
    async initialize() {
      try {
        await axios
          .get('http://localhost:8000/api/getalluser')
          .then((response) => {
            // console.log(response.data)
            this.desserts = response.data
          })
          .catch((error) => console.log(error))
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
