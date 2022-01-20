<template>
  <v-container>
    <v-card-title class="mt-5 justify-center"
      ><h1 class="display-2 font-weight-bold mb-3">
        Simple Crypto Dashboard
      </h1></v-card-title
    >
    <v-card class="mt-5">
      <v-card-title>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="coins"
        :search="search"
        :loading="tableIsLoading"
        loading-text="Loading data... Please wait"
      >
        <template v-slot:item.image="{ item }">
          <div class="d-flex justify-center">
            <v-img
              max-height="20"
              max-width="20"
              v-model="item.image"
              :src="item.image"
            ></v-img>
          </div>
        </template>

        <template v-slot:item.current_price="{ item }">
          $
          {{
            (Math.round(item.current_price * 100) / 100)
              .toString()
              .replace(/\B(?=(\d{3})+(?!\d))/g, ",")
          }}
        </template>

        <template v-slot:item.market_cap="{ item }">
          $
          {{
            (Math.round(item.market_cap * 100) / 100)
              .toString()
              .replace(/\B(?=(\d{3})+(?!\d))/g, ",")
          }}
        </template>

        <template v-slot:item.price_change_percentage_24h="{ item }">
          {{
            (Math.round(item.price_change_percentage_24h * 10) / 10)
              .toString()
              .replace(/\B(?=(\d{3})+(?!\d))/g, ",")
          }}%
        </template>
      </v-data-table>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: "CryptoDashboard",
  async created() {
    // fetch the coins from coingecko
    const response = await this.axios.get(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&sparkline=false"
    );
    this.coins = response.data;
    this.tableIsLoading = false;
    console.log(this.coins);
  },
  data() {
    return {
      tableIsLoading: true,
      search: "",
      headers: [
        {
          text: "",
          align: "center",
          filterable: false,
          value: "image",
        },
        {
          text: "Name",
          align: "start",
          filterable: false,
          value: "name",
        },
        {
          text: "Price",
          align: "end",
          filterable: false,
          value: "current_price",
        },
        {
          text: "24h",
          align: "end",
          filterable: false,
          value: "price_change_percentage_24h",
        },

        {
          text: "Market Cap",
          align: "end",
          filterable: false,
          value: "market_cap",
        },
      ],
      coins: [], // will be loaded dynamically
    };
  },
};
</script>

<style scoped>
</style>