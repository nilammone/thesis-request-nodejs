<template>
  <div class="mt-3 container">
    <h3 class="mytxt"><v-icon>mdi-file-multiple</v-icon> ລາຍງານຍ່ອຍ</h3>
    <hr class="primary" />
    <v-row class="mt-6">
      <v-col cols="12" md="9">
        <v-card>
          <v-form>
            <v-row class="ml-6">
              <v-col cols="12" md="4">
                <v-text-field
                  v-model="sdate"
                  type="date"
                  label="ເລີ່ມວັນທີ"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="4">
                <v-text-field
                  v-model="edate"
                  type="date"
                  label="ເຖິງວັນທີ"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="4" class="mt-4">
                <v-btn color="success">ຄົ້ນຫາ</v-btn>
              </v-col>
            </v-row>
          </v-form>
        </v-card>
      </v-col>

      <v-col cols="12" md="3" class="ml-auto">
        <v-navigation-drawer permanent>
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title style="color: #2e5fb0">
                ລາຍງານຍ່ອຍ
              </v-list-item-title>
              <v-list-item-subtitle> ເລືອກລາຍງານຂອງທ່ານ </v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-divider></v-divider>

          <v-list dense nav>
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title>ຊັບສິນ</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title>ອຸປະກອນ</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title>ອາຄານ</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title>ຫ້ອງ</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item link>
              <v-list-item-content>
                <v-list-item-title>ຜູ້ສະຫນອງ</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-navigation-drawer>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sdate: '',
      edate: '',
    }
  },
}
</script>
