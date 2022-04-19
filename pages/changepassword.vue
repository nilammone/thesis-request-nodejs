<template>
  <div class="mt-3 container">
    <h3 class="mytxt"><v-icon>mdi-lock-reset</v-icon> ປ່ຽນລະຫັດຜ່ານ</h3>
    <hr />
    <form class="mt-4" @submit.prevent="submit">
      <v-row>
        <v-col cols="4">
          <v-text-field
            v-model="curpass"
            type="password"
            label="Current Password"
            required
          ></v-text-field>

          <v-text-field
            v-model="newpass"
            type="password"
            label="New Password"
            required
          ></v-text-field>

          <v-text-field
            v-model="connewpass"
            type="password"
            label="Confirm Password"
            required
          ></v-text-field>
        </v-col>
        <v-col cols="4">&nbsp;</v-col>
        <v-col cols="4">&nbsp;</v-col>
      </v-row>

      <v-btn class="mr-4 info" type="submit"> ບັນທຶກ </v-btn>
      <v-alert v-if="shmessage" class="mt-8" :type="chcolormsg">{{
        shmessage
      }}</v-alert>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      curpass: '',
      newpass: '',
      connewpass: '',
      shmessage: '',
      chcolormsg: '',
    }
  },

  methods: {
    async submit() {
      const sendData = {
        user_id: this.$auth.user.id,
        old_password: this.curpass,
        new_password: this.newpass,
        confirm_password: this.connewpass,
      }

      try {
        await this.$axios.post('/change_password', sendData).then((res) => {
          this.shmessage = res.data.message

          if (res.data.status == 200) {
            this.chcolormsg = 'success'
            this.$auth.logout()
          } else if (res.data.status == 400) {
            this.chcolormsg = 'error'
          }
        })
      } catch (err) {
        console.log(err)
      }
    },
  },
}
</script>
