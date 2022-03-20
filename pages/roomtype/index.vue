<template>
  <div class="mt-3 container">
    <h3 class="mytxt"><v-icon>mdi-google-classroom</v-icon> ຂໍ້ມູນປະເພດຫ້ອງ</h3>
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
          <v-toolbar-title>Type Room</v-toolbar-title>
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
                    <v-col cols="12" sm="12" md="12">
                      <v-text-field
                        v-model="editedItem.rt_name"
                        label="ປະເພດຫ້ອງ"
                        :rules="nameRules"
                      ></v-text-field>
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
                  @click="deleteItemConfirm(editedItem.rt_id)"
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
      nameRules: [
        (v) => !!v || 'Room_type_name is required',
        (v) =>
          v.length <= 30 || 'Room_type_name must be less than 30 characters',
      ],
      headers: [
        {
          text: 'ລຳດັບ',
          align: 'center',
          sortable: false,
          value: 'numlist',
        },
        {
          text: 'ຊື່ປະເພດຫ້ອງ',
          value: 'rt_name',
          align: 'center',
        },

        { text: 'ຈັດການ', value: 'actions', sortable: false, align: 'center' },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        rt_name: '',
      },
      defaultItem: {
        rt_name: '',
      },
    }
  },

  computed: {
    formTitle() {
      return this.editedIndex === -1
        ? 'ເພີ່ມຂໍ້ມູນປະເພດຫ້ອງ'
        : 'ແກ້ໄຂຂໍ້ມູນປະເພດຫ້ອງ'
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
        await this.$axios.get('/roomtypes').then((res) => {
          this.desserts = res.data
        })
      } catch (error) {
        console.log(error)
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

    async deleteItemConfirm(rtId) {
      try {
        await this.$axios.delete(`/roomtypes/${rtId}`).then((res) => {
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
          rt_name: this.editedItem.rt_name,
        }

        try {
          await this.$axios
            .put(`/roomtypes/${this.editedItem.rt_id}`, sendresdata)
            .then((res) => {
              console.log('edit completed!')
            })

          this.initialize()
        } catch (err) {
          console.log(err)
        }
      } else {
        const getresdata = {
          rt_name: this.editedItem.rt_name,
        }

        try {
          await this.$axios.post('/roomtypes', getresdata).then((res) => {
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
