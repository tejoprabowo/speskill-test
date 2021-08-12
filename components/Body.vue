<template>
  <div>
    <v-container>
      <v-row justify="center" align="center">
        <v-col xs="12" sm="12" md="8" lg="8" xl="8">
          <h1 class="text-center">SPE Frontend Shop</h1>
        </v-col>
      </v-row>

      <v-row justify="center" align="center">
        <v-col xs="12" sm="12" md="10" lg="10" xl="10">
          <v-row class="white--text">
            <v-col style="background-color: #111111;" class="text-center" xs="12" sm="12" md="8" lg="8" xl="8">
              PRODUCT
            </v-col>
            <v-col style="background-color: #111111;" class="text-center" xs="12" sm="12" md="2" lg="2" xl="2">
              QUANTITY
            </v-col>
            <v-col style="background-color: #111111;" class="text-center" xs="12" sm="12" md="2" lg="2" xl="2">
              SUBTOTAL
            </v-col>
          </v-row>
        </v-col>
      </v-row>

      <v-row justify="center" align="center" v-for="data in dataShop" :key="data.product.code">
        <v-col xs="12" sm="12" md="10" lg="10" xl="10">

          <v-row>
            <v-col xs="12" sm="12" md="8" lg="8" xl="8" style="background-color: white;">
              <v-row>
                <v-col xs="12" sm="12" md="4" lg="4" xl="4">
                  <v-img
                    :src="data.product.media_url"
                  />
                </v-col>
                <v-col xs="12" sm="12" md="7" lg="7" xl="7">
                  <span class="blue--text" style="font-size: 12px;">{{ data.product.code }}</span> <br />
                  <span class="black--text" style="font-size: 14px;">{{ data.product.name }}</span> <br />
                  <span class="grey--text" style="font-size: 14px;">{{ convertToRupiah(data.product.price) }}</span> <br />
                  <span class="red--text" style="font-size: 10px;">{{ data.product.stock }} in stock</span>
                </v-col>
              </v-row>
            </v-col>

            <v-col xs="12" sm="12" md="2" lg="2" xl="2" style="background-color: white;">
              <v-text-field outlined type="number" v-model="data.product.stockInput" @change="changeSubTotal(data.product)" />
            </v-col>

            <v-col xs="12" sm="12" md="2" lg="2" xl="2" style="background-color: white;">
              <v-text-field style="color: red !important;" v-model="data.product.idr" disabled outlined type="text" />
            </v-col>
          </v-row>

        </v-col>
      </v-row>

      <v-row justify="center" align="center">
        <v-col xs="12" sm="12" md="10" lg="10" xl="10">
          <v-row class="white--text">
            <v-col style="background-color: #111111;" class="text-center" xs="12" sm="12" md="6" lg="6" xl="6">
            </v-col>
            <v-col style="background-color: #111111;" class="text-center" xs="12" sm="12" md="2" lg="2" xl="2">
            </v-col>
            <v-col style="background-color: #111111;" class="text-center" xs="12" sm="12" md="4" lg="4" xl="4">
              TOTAL: {{ this.totalPrice() }}
            </v-col>
          </v-row>
        </v-col>
      </v-row>

    </v-container>
  </div>
</template>

<script>

export default {
  data: () => ({
    dataShop: [],
    stockInput: null
  }),

  methods: {
    async getDataShop() {
      const { data } = await this.$axios.get('https://spe-academy.spesolution.net/api/recruitment', {
        headers: {
          "Content-Type": "application/json",
          "Authorization": "Bearer " + "o7Ytbt9XQLI3PgtebJfKSXKEf0XHU74Y"
        }
      });
      // console.log(data)
      for (const dataProduct in data) {
        data[dataProduct].product.subTotal = 0
        data[dataProduct].product.stockInput = 0
        data[dataProduct].product.idr = null
      }
      this.dataShop = data
    },

    changeSubTotal(data) {
      data.subTotal = parseInt(data.stockInput) * parseInt(data.price)
      data.idr = this.convertToRupiah(parseInt(data.stockInput) * parseInt(data.price))
    },

    totalPrice() {
      let total = []
      for (const data in this.dataShop) {
        total.push(this.dataShop[data].product.subTotal)
      }
      console.log(total)
    },

    convertToRupiah(angka) {
      var rupiah = '';		
      var angkarev = angka.toString().split('').reverse().join('');
      for(var i = 0; i < angkarev.length; i++) if(i%3 == 0) rupiah += angkarev.substr(i,3)+',';
      return 'IDR. '+rupiah.split('',rupiah.length-1).reverse().join('');
    }
  },

  mounted() {
    this.getDataShop()
  } 
}
</script>
