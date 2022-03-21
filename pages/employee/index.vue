<template>
  <div class="mt-3 container">
    <h3 class="mytxt">
      <v-icon>mdi-account-box-multiple</v-icon> ຂໍ້ມູນພະນັກງານ
    </h3>
    <hr />

    <v-data-table
      :headers="headers"
      :items="desserts"
      :search="search"
      sort-by=""
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
                        v-model="editedItem.emp_firstname"
                        label="ຊື່
              "
                        :rules="nameRules"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.emp_lastname"
                        label="ນາມສະກຸນ"
                        :rules="nameRules"
                      ></v-text-field>
                    </v-col>
                    <!-- <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.emp_dept_id"
                        label="deptid"
                      ></v-text-field>
                    </v-col> -->
                    <v-col cols="12" sm="6" md="6">
                      <v-select
                        v-model="editedItem.emp_dept_id"
                        :items="itemsemps"
                        item-value="dept_id"
                        item-text="dept_name"
                        :rules="[(v) => !!v || 'Item is required']"
                        label="ພະແນກ"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.emp_contact"
                        label="ຕິດຕໍ່"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-text-field
                        v-model="editedItem.emp_address"
                        label="ທີ່ຢູ່"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="6">
                      <v-file-input
                        accept="image/*"
                        v-model="editedItem.image"
                        label="ຮູບພາບ"
                      ></v-file-input>
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
                  @click="deleteItemConfirm(editedItem.emp_id)"
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

      <template #[`item.emp_image`]="{ item }">
        <v-avatar>
          <img
            :src="`http://localhost:8000/storage/${item.emp_image}`"
            alt=""
            width="50"
            height="50"
          />
        </v-avatar>
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
    <!-- <v-snackbar v-model="snackbar" absolute bottom color="success" outlined>
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn color="success" text v-bind="attrs" @click="snackbar = false">
          <v-icon>mdi-close-circle</v-icon>
        </v-btn>
      </template>
    </v-snackbar> -->
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
      carouselInterval: null,
      getupfilename: null,
      multiLine: true,
      snackbar: false,
      text: `Success!.`,
      search: '',
      dialog: false,
      dialogDelete: false,
      nameRules: [
        (v) => !!v || 'required',
        (v) => v.length <= 30 || 'Must be less than 30 characters',
      ],
      itemsemps: [],
      headers: [
        {
          text: 'ລຳດັບ',
          align: 'center',
          sortable: false,
          value: 'numlist',
        },
        { text: 'ຊື່', value: 'emp_firstname', align: 'center' },
        { text: 'ນາມສະກຸນ', value: 'emp_lastname', align: 'center' },
        { text: 'ພະແນກ', value: 'dept_name', align: 'center' },
        { text: 'ຕິດຕໍ່', value: 'emp_contact', align: 'center' },
        { text: 'ທີ່ຢູ່', value: 'emp_address', align: 'center' },
        { text: 'ຮູບພາບ', value: 'emp_image', align: 'center' },
        { text: 'ຈັດການ', value: 'actions', sortable: false, align: 'center' },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        emp_firstname: '',
        emp_lastname: '',
        emp_dept_id: 0,
        emp_contact: '',
        emp_address: '',
        emp_image: '',
      },
      defaultItem: {
        emp_firstname: '',
        emp_lastname: '',
        emp_dept_id: 0,
        emp_contact: '',
        emp_address: '',
        emp_image: '',
      },
    }
  },

  computed: {
    formTitle() {
      return this.editedIndex === -1
        ? 'ເພີ່ມ ຂໍ້ມູນພະນັກງານ'
        : 'ແກ້ໄຂ ຂໍ້ມູນພະນັກງານ'
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
    this.getDatadepartments()
  },

  methods: {
    async initialize() {
      // s set api
      try {
        await this.$axios.get(`/getdataJoindepartments`).then((res) => {
          this.desserts = res.data
        })
      } catch (err) {
        console.log(err)
      }
      // e set api
    },

    // get data department
    async getDatadepartments() {
      try {
        await this.$axios.get('/departments').then((res) => {
          this.itemsemps = res.data
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

    async deleteItemConfirm(empid) {
      // s delete
      try {
        await this.$axios
          .delete(`/employees/${empid}`)
          .then((res) => {
            console.log('Delete completed!')
          })
          .catch((err) => {
            console.log(err)
          })

        // delete image
        await this.$axios
          .get(`deleteimage/${this.editedItem.emp_image}`)
          .then((res) => {
            console.log('Delete image completed!')
          })
          .catch((err) => {
            console.log(err)
          })

        this.initialize()
        this.dialogDelete = false
        // console.log(this.editedItem.emp_image)
      } catch (error) {
        console.log(error)
      }
      // e delete
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

    async save() {
      if (this.editedIndex > -1) {
        if (this.editedItem.image === undefined) {
          const sendresdata = {
            emp_firstname: this.editedItem.emp_firstname,
            emp_lastname: this.editedItem.emp_lastname,
            emp_dept_id: this.editedItem.emp_dept_id,
            emp_contact: this.editedItem.emp_contact,
            emp_address: this.editedItem.emp_address,
            emp_image: this.editedItem.emp_image,
          }

          try {
            await this.$axios
              .put(`/employees/${this.editedItem.emp_id}`, sendresdata)
              .then((res) => {
                console.log('edit completed!')
              })

            this.initialize()
          } catch (err) {
            console.log(err)
          }
        } else {
          // for edit add new image
          // delete image
          await this.$axios
            .get(`deleteimage/${this.editedItem.emp_image}`)
            .then((res) => {
              console.log('Delete image completed!')
            })
            .catch((err) => {
              console.log(err)
            })

          // s upload image
          const fd = new FormData()
          fd.append('file', this.editedItem.image, this.editedItem.image.name)

          await this.$axios
            .post('/upload', fd)
            .then((res) => {
              console.log('upload image completed!')
              this.getupfilename = res.data.result
            })
            .catch((err) => {
              console.log(err)
            })
          // e upload image

          // cut name image
          const subimagename = this.getupfilename.substr(7)

          const sendresdata = {
            emp_firstname: this.editedItem.emp_firstname,
            emp_lastname: this.editedItem.emp_lastname,
            emp_dept_id: this.editedItem.emp_dept_id,
            emp_contact: this.editedItem.emp_contact,
            emp_address: this.editedItem.emp_address,
            emp_image: subimagename,
          }

          try {
            await this.$axios
              .put(`/employees/${this.editedItem.emp_id}`, sendresdata)
              .then((res) => {
                console.log('edit completed!')
              })

            this.initialize()
          } catch (err) {
            console.log(err)
          }
        }
      } else {
        // s upload image
        const fd = new FormData()
        fd.append('file', this.editedItem.image, this.editedItem.image.name)

        await this.$axios
          .post('/upload', fd)
          .then((res) => {
            // console.log(res.data.result)
            console.log('upload image completed!')
            this.getupfilename = res.data.result
          })
          .catch((err) => {
            console.log(err)
          })
        // e upload image

        // cut name image
        const subimagename = this.getupfilename.substr(7)

        // s insert
        const getresdata = {
          emp_firstname: this.editedItem.emp_firstname,
          emp_lastname: this.editedItem.emp_lastname,
          emp_dept_id: this.editedItem.emp_dept_id,
          emp_contact: this.editedItem.emp_contact,
          emp_address: this.editedItem.emp_address,
          emp_image: subimagename,
        }

        try {
          await this.$axios
            .post('/employees', getresdata)
            .then((res) => {
              console.log('Insert completed!')
            })
            .catch((error) => console.log(error))

          this.carouselInterval = setInterval(() => {
            this.snackbar = true
          }, 800)

          this.initialize()

          this.snackbar = false
        } catch (err) {
          console.log(err)
        }
        // e insert
      }
      this.close()
    },
  },
}
</script>

<style scoped></style>
