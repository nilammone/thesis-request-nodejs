<template>
  <div>
    <h3 class="text-center">Create</h3>
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
            <v-text-field v-model="phone" label="Phone" required></v-text-field>
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
      </v-col>
      <v-col cols="4">&nbsp;</v-col>
    </v-row>
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
        })
        .catch((err) => {
          console.log(err)
        })
    },
  },
}
</script>

<style></style>
