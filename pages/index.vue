<template>
  <div class="mt-3">
    <h3 class="mytxt">ພາບລວມຂໍ້ມູນຊັບສິນທັງຫມົດ</h3>
    <hr />

    <div class="mt-5">
      <v-row>
        <v-col md="3">
          <v-card class="mx-auto" max-width="100%">
            <v-img
              src="https://cdn.pixabay.com/photo/2016/11/22/19/24/archive-1850170__340.jpg"
              height="200px"
            ></v-img>

            <v-card-title> ຈຳນວນຊັບສິນທັງຫມົດ </v-card-title>

            <v-card-subtitle> 1,000 </v-card-subtitle>
          </v-card>
        </v-col>
        <v-col md="3">
          <v-card class="mx-auto" max-width="100%">
            <v-img
              src="https://cdn.pixabay.com/photo/2019/05/29/00/43/johns-hopkins-university-4236470__340.jpg"
              height="200px"
            ></v-img>

            <v-card-title> ຈຳນວນອາຄານທັງຫມົດ </v-card-title>

            <v-card-subtitle> 10 </v-card-subtitle>
          </v-card>
        </v-col>
        <v-col md="3">
          <v-card class="mx-auto" max-width="100%">
            <v-img
              src="https://cdn.pixabay.com/photo/2012/09/10/05/10/steinbach-56641__340.jpg"
              height="200px"
            ></v-img>

            <v-card-title> ຈຳນວນຫ້ອງຣຽນທັງຫມົດ </v-card-title>

            <v-card-subtitle> 200 </v-card-subtitle>
          </v-card>
        </v-col>
        <v-col md="3">
          <v-card class="mx-auto" max-width="100%">
            <v-img
              src="https://cdn.pixabay.com/photo/2020/11/23/03/56/handshake-5768632__340.jpg"
              height="200px"
            ></v-img>

            <v-card-title> ຈຳນວນຜູ້ສະຫນອງທັງຫມົດ </v-card-title>

            <v-card-subtitle> 10 </v-card-subtitle>
          </v-card>
        </v-col>
      </v-row>
    </div>

    <!-- chart -->
    <div class="mt-5">
      <v-row>
        <v-col md="4">
          <v-card color="white">
            <apexchart
              width="500"
              type="bar"
              :options="options"
              :series="series"
            ></apexchart>
          </v-card>
        </v-col>
        <v-col md="4">
          <v-card color="white">
            <apexchart
              width="500"
              type="line"
              :options="options2"
              :series="series2"
            ></apexchart> </v-card
        ></v-col>
        <v-col md="4">
          <v-card color="white" class="pa-10">
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
          categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998],
        },
      },
      series: [
        {
          name: 'series-1',
          data: [30, 40, 45, 50, 49, 60, 70, 91],
        },
      ],

      options3: {},
      series3: [44, 55, 41, 17, 15],

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
    }
  },
}
</script>

<style scoped>
.chbg {
  background-color: white;
}
</style>
