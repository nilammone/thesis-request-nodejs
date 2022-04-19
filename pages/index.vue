<template>
  <div class="mt-3">
    <h3 class="mytxt">ພາບລວມຂໍ້ມູນຊັບສິນທັງຫມົດ</h3>
    <hr />

    <div class="mt-5">
      <v-row>
        <v-col md="3">
          <v-card class="mx-auto" max-width="100%">
            <v-img src="/allasset.webp" height="200px"></v-img>

            <v-row align="center" justify="end">
              <v-col md="8"
                ><v-card-title class="myft10" align="right">
                  ຈຳນວນຊັບສິນທັງຫມົດ
                </v-card-title></v-col
              >
              <v-col md="4"
                ><v-card-subtitle>
                  {{ countAssets }}
                </v-card-subtitle></v-col
              >
            </v-row>
          </v-card>
        </v-col>
        <v-col md="3">
          <v-card class="mx-auto" max-width="100%">
            <v-img src="/bb20220307.jpg" height="200px"></v-img>

            <v-row align="center" justify="end">
              <v-col md="8"
                ><v-card-title class="myft10" align="right">
                  ຈຳນວນອາຄານທັງຫມົດ
                </v-card-title></v-col
              >
              <v-col md="4"
                ><v-card-subtitle>
                  {{ countBuildings }}
                </v-card-subtitle></v-col
              >
            </v-row>
          </v-card>
        </v-col>
        <v-col md="3">
          <v-card class="mx-auto" max-width="100%">
            <v-img src="/room20220307.jpg" height="200px"></v-img>

            <v-row align="center" justify="end">
              <v-col md="8"
                ><v-card-title class="myft10" align="right">
                  ຈຳນວນຫ້ອງຣຽນທັງຫມົດ
                </v-card-title></v-col
              >
              <v-col md="4"
                ><v-card-subtitle> {{ countRooms }} </v-card-subtitle></v-col
              >
            </v-row>
          </v-card>
        </v-col>
        <v-col md="3">
          <v-card class="mx-auto" max-width="100%">
            <v-img src="vendor.webp" height="200px"></v-img>

            <v-row align="center" justify="end">
              <v-col md="8"
                ><v-card-title class="myft10" align="right">
                  ຈຳນວນຜູ້ສະຫນອງທັງຫມົດ
                </v-card-title></v-col
              >
              <v-col md="4"
                ><v-card-subtitle>
                  {{ countSuppilers }}
                </v-card-subtitle></v-col
              >
            </v-row>
          </v-card>
        </v-col>
      </v-row>
    </div>

    <!-- chart -->
    <div class="mt-5">
      <v-row>
        <v-col md="6">
          <v-card color="white">
            <apexchart
              width="500"
              type="bar"
              :options="options"
              :series="series"
            ></apexchart>
          </v-card>
        </v-col>
        <!-- <v-col md="4">
          <v-card color="white">
            <apexchart
              width="500"
              type="line"
              :options="options2"
              :series="series2"
            ></apexchart> </v-card
        ></v-col> -->
        <v-col md="6">
          <v-card color="white">
            <apexchart
              width="380"
              type="donut"
              :options="options3"
              :series="series3"
            ></apexchart> </v-card
        ></v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      options: {
        chart: {
          id: 'vuechart-example',
        },
        xaxis: {
          categories: [2020, 2021, 2022],
        },
      },
      series: [
        {
          name: 'ຈຳນວນຊັບສິນ',
          data: [30, 40, 45],
        },
      ],

      options3: {
        labels: ['ໂຕະ', 'ຄອມພິວເຕີ', 'ອຸປະກອນທົດລອງ'],
      },
      series3: [10, 5, 3],

      options2: {
        chart: {
          id: 'vuechart-example2',
        },
        xaxis: {
          categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998],
        },
      },
      series2: [
        {
          name: 'series-2',
          data: [30, 40, 45, 50, 49, 60, 70, 91],
        },
      ],

      countAssets: null,
      countBuildings: null,
      countRooms: null,
      countSuppilers: null,
    }
  },

  created() {
    this.getDataCountAllAsset()
  },

  methods: {
    async getDataCountAllAsset() {
      try {
        await this.$axios.get('/countAllAssets').then((res) => {
          this.countAssets = res.data[0].totalAssets
        })

        await this.$axios.get('/countAllBuildings').then((res) => {
          this.countBuildings = res.data[0].totalBuilding
        })

        await this.$axios.get('/countAllRooms').then((res) => {
          this.countRooms = res.data[0].totalRooms
        })

        await this.$axios.get('/countAllSuppilers').then((res) => {
          this.countSuppilers = res.data[0].totalSuppilers
        })
      } catch (err) {
        console.log(err)
      }
    },
  },
}
</script>

<style scoped>
.chbg {
  background-color: white;
}

.myft10 {
  font-size: 1em;
}
</style>
