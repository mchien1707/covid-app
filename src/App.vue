<template>
  <div id="app">
    <!-- <GChart
      type="GeoChart"
      :data="chartData"
      :options="chartOptions"
      :settings="{ packages: ['corechart', 'map', 'geochart'] }"
    /> -->
    <GChart
      type="GeoChart"
      :data="chartData"
      :options="chartOptions"
      :settings="{ packages: ['geochart'] }"
    />
    <!-- <GChart
      type="PieChart"
      :data="chartData"
      :options="chartOptions"
      :settings="{ packages: ['corechart'] }"
    /> -->
  </div>
</template>

<script>
import { GChart } from "vue-google-charts";
import axios from "axios";
export default {
  name: "App",
  data: () => {
    return {
      chartData: [["CountryID", "Country", "Total", "Death"]],
      chartOptions: {
        chart: {
          title: "Covid Tracking",
          subtitle: "",
        },
        colorAxis: { colors: ["7bcefa", "ff9002", "f5500c"] },
      },
      width: 1000
    };
  },
  components: {
    GChart,
  },
  created() {
    this.getData();
  },
  methods: {
    async getData() {
      const data = await axios.get("https://corona.lmao.ninja/v2/countries"); /*axios.get("https://api.covid19api.com/summary");*/
      let chartValue = [];
      data.data.map((item) => {
        let country = [];
        country.push(
          item.countryInfo.iso2,
          item.country,
          item.cases,
          item.deaths
        );
        chartValue.push(country);
      });

      this.chartData = [...this.chartData, ...chartValue];
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
