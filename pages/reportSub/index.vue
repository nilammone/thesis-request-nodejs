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

        <!-- s table -->
        <v-card class="mt-6">
          <v-card-title>
            Report details
            <v-spacer></v-spacer>
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
          </v-card-title>
          <v-data-table :headers="headers" :items="desserts" :search="search">
            <template #[`item.numlist`]="{ item }">
              {{
                desserts
                  .map(function (x) {
                    return x.id
                  })
                  .indexOf(item.id) + 1
              }}
            </template>
          </v-data-table>
        </v-card>
        <!-- e table -->
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
      search: '',
      headers: [
        {
          text: 'No',
          align: 'center',
          sortable: false,
          value: 'numlist',
        },
        { text: 'Head01', value: 'calories' },
        { text: 'Head02', value: 'fat' },
        { text: 'Head03', value: 'carbs' },
        { text: 'Head04', value: 'protein' },
        { text: 'Head05', value: 'iron' },
      ],
      desserts: [
        {
          id: 10,
          name: 'Frozen Yogurt',
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          iron: '1%',
        },
        {
          id: 20,
          name: 'Ice cream sandwich',
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          iron: '1%',
        },
        {
          id: 30,
          name: 'Eclair',
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          iron: '7%',
        },
        {
          id: 40,
          name: 'Cupcake',
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          iron: '8%',
        },
        {
          id: 50,
          name: 'Gingerbread',
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          iron: '16%',
        },
        {
          id: 60,
          name: 'Jelly bean',
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          iron: '0%',
        },
        {
          id: 70,
          name: 'Lollipop',
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          iron: '2%',
        },
        {
          id: 80,
          name: 'Honeycomb',
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          iron: '45%',
        },
        {
          id: 90,
          name: 'Donut',
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          iron: '22%',
        },
        {
          id: 100,
          name: 'KitKat',
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          iron: '6%',
        },
      ],
    }
  },
}
</script>
