<template>
  <div>
    <!-- s create records -->
    <div>
      <h3 class="text-center">Create Data (Node Js)</h3>
      <hr />
      <v-row>
        <v-col cols="4">&nbsp;</v-col>
        <v-col cols="4" class="mt-4">
          <v-card class="pa-6">
            <form>
              <v-text-field
                v-model="first_name"
                label="First_name"
                required
              ></v-text-field>
              <v-text-field
                v-model="last_name"
                label="Last_name"
                required
              ></v-text-field>
              <v-text-field
                v-model="birth_date"
                type="date"
                label="Birth_date"
                required
              ></v-text-field>
              <v-text-field
                v-model="e_mail"
                :rules="emailRules"
                label="E_mail"
                required
              ></v-text-field>
              <v-text-field
                v-model="phone"
                label="Phone"
                required
              ></v-text-field>
              <v-text-field
                v-model="address"
                label="Address"
                required
              ></v-text-field>

              <!-- s btn -->
              <v-row>
                <v-col cols="8">
                  <v-text-field
                    v-model="numRecord"
                    label="Number of record to insert:"
                    required
                  ></v-text-field
                ></v-col>
                <v-col cols="4">
                  <v-btn class="mt-2" @click="submitAdd()"> Add </v-btn></v-col
                >
              </v-row>

              <!-- e btn -->
            </form>
          </v-card>

          <!-- s alert add -->
          <v-alert v-if="chalertAdd" dense text type="success" class="mt-6">
            {{ numRecord }} Records Inserted Successfully!
          </v-alert>
          <!-- e alert add -->
        </v-col>
        <v-col cols="4">&nbsp;</v-col>
      </v-row>
    </div>
    <!-- e create records -->

    <!-- s get records -->
    <div>
      <h3 class="text-center mymt">Get Data (Node Js)</h3>
      <hr />
      <v-row>
        <v-col cols="4">&nbsp;</v-col>
        <v-col cols="4" class="mt-4">
          <v-card class="pa-6">
            <form>
              <!-- s btn -->
              <v-row>
                <v-col cols="8">
                  <v-text-field
                    v-model="numRecordGet"
                    label="Number of record to Get:"
                    required
                  ></v-text-field
                ></v-col>
                <v-col cols="4">
                  <v-btn class="mt-2" @click="submitGet()"> Get </v-btn></v-col
                >
              </v-row>

              <!-- e btn -->
            </form>
          </v-card>

          <!-- s alert add -->
          <v-alert v-if="chalertGet" dense text type="success" class="mt-6">
            {{ numRecord }} Records Selected Successfully!
          </v-alert>
          <!-- e alert add -->
        </v-col>
        <v-col cols="4">&nbsp;</v-col>
      </v-row>
    </div>
    <!-- e get records -->

    <!-- s update records -->
    <div>
      <h3 class="text-center mymt">Update Data (Node Js)</h3>
      <hr />
      <v-row>
        <v-col cols="4">&nbsp;</v-col>
        <v-col cols="4" class="mt-4">
          <v-card class="pa-6">
            <form>
              <v-text-field
                v-model="first_name_update"
                label="First_name"
                required
              ></v-text-field>
              <!-- s btn -->
              <v-row>
                <v-col cols="8">
                  <v-text-field
                    v-model="numRecordUpdate"
                    label="Number of record to Update:"
                    required
                  ></v-text-field
                ></v-col>
                <v-col cols="4">
                  <v-btn class="mt-2" @click="submitUpdate()">
                    Update
                  </v-btn></v-col
                >
              </v-row>

              <!-- e btn -->
            </form>
          </v-card>

          <!-- s alert add -->
          <v-alert v-if="chalertUpdate" dense text type="success" class="mt-6">
            {{ numRecord }} Records Updated Successfully!
          </v-alert>
          <!-- e alert add -->
        </v-col>
        <v-col cols="4">&nbsp;</v-col>
      </v-row>
    </div>
    <!-- e update records -->

    <!-- s delete records -->
    <div style="margin-bottom: 500px">
      <h3 class="text-center mymt">Delete Data (Node Js)</h3>
      <hr />
      <v-row>
        <v-col cols="4">&nbsp;</v-col>
        <v-col cols="4" class="mt-4">
          <v-card class="pa-6">
            <form>
              <!-- s btn -->
              <v-row>
                <v-col cols="8">
                  <v-text-field
                    v-model="numRecordDelete"
                    label="Number of record to Delete:"
                    required
                  ></v-text-field
                ></v-col>
                <v-col cols="4">
                  <v-btn class="mt-2" @click="submitDelete()">
                    Delete
                  </v-btn></v-col
                >
              </v-row>

              <!-- e btn -->
            </form>
          </v-card>

          <!-- s alert add -->
          <v-alert v-if="chalertDelete" dense text type="success" class="mt-6">
            {{ numRecord }} Records Deleted Successfully!
          </v-alert>
          <!-- e alert add -->
        </v-col>
        <v-col cols="4">&nbsp;</v-col>
      </v-row>
    </div>
    <!-- e delete records -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      first_name: '',
      last_name: '',
      birth_date: '',
      phone: '',
      address: '',
      numRecord: '',
      e_mail: '',
      emailRules: [
        (v) => !!v || 'E-mail is required',
        (v) => /.+@.+/.test(v) || 'E-mail must be valid',
      ],

      chalertAdd: false,
      chalertGet: false,

      numRecordGet: '',

      first_name_update: '',
      numRecordUpdate: '',
      chalertUpdate: false,

      numRecordDelete: '',
      chalertDelete: false,
    }
  },
  methods: {
    async submitAdd() {
      console.log(
        this.first_name +
          ' ' +
          this.last_name +
          ' ' +
          this.birth_date +
          ' ' +
          this.e_mail +
          ' ' +
          this.phone +
          ' ' +
          this.address +
          ' ' +
          this.numRecord
      )

      const sdata = {
        first_name: this.first_name,
        last_name: this.last_name,
        birth_date: this.birth_date,
        e_mail: this.e_mail,
        phone: this.phone,
        address: this.address,
      }

      await this.$axios
        .post(
          `http://localhost:5533/api/v1/customers/multi/${this.numRecord}`,
          sdata
        )
        .then((res) => {
          console.log('Insert completed!')
          this.chalertAdd = true
        })
        .catch((err) => {
          console.log(err)
        })
    },

    async submitGet() {
      console.log('---- Get Data ----')
      await this.$axios
        .get(
          `http://localhost:5533/api/v1/customers/bylimit/${this.numRecordGet}`
        )
        .then((res) => {
          //   console.dir(res.data)
          console.log('Get data Completed!')
          this.chalertGet = true
        })
        .catch((err) => {
          console.log(err)
        })
    },

    async submitUpdate() {
      console.log('---- Update Data ----')
      const supdatedata = {
        first_name: this.first_name_update,
      }

      await this.$axios
        .put(
          `http://localhost:5533/api/v1/customers/${this.numRecordUpdate}`,
          supdatedata
        )
        .then((res) => {
          //   console.log(res.data)
          console.log('Update data Completed!')
          this.chalertUpdate = true
        })
        .catch((err) => {
          console.log(err)
        })
    },

    async submitDelete() {
      console.log('---- Delete Data ----')
      await this.$axios
        .delete(
          `http://localhost:5533/api/v1/customers/${this.numRecordDelete}`
        )
        .then((res) => {
          console.log('Delete Completed!')
          this.chalertDelete = true
        })
        .catch((err) => {
          console.log(err)
        })
    },
  },
}
</script>

<style>
.mymt {
  margin-top: 120px;
}
</style>
