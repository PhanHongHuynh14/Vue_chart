<template>
  <div class="chart-container">
    <h4>Free Members By Section</h4>
    <div class="card-body">
      <div id="chartId"></div>
    </div>
  </div>
</template>

<script>
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";
import axios from 'axios';

export default {
  props: {
    chartTitle: String,
    chartId: String
  },
  data() {
    return {
      chartData: [],
    };
  },
  mounted() {
    this.fetchData();
  },

  methods: {
    async fetchData() {
      try {
        const res = await axios.get('http://localhost:8000/api/worker', { withCredentials: true });
        this.chartData = res.data.data;
        this.createChart();
      } catch (error) {
        console.error('Error fetching data from API:', error);
      }
    },

    createChart() {
      const uniqueSections = [...new Set(this.chartData.map(employee => employee.section_name))];
      const sectionData = [];

      uniqueSections.forEach(section => {
        const filteredData = this.chartData.filter(employee => employee.section_name === section);
        sectionData.push({
          category: section,
          value: filteredData.length,
        });
      });

      const chart = am4core.create("chartId", am4charts.PieChart);
      chart.data = sectionData;

      const pieSeries = chart.series.push(new am4charts.PieSeries());
      pieSeries.dataFields.value = "value";
      pieSeries.dataFields.category = "category";
      pieSeries.slices.template.fill = am4core.colorSet;
      pieSeries.labels.template.wrap = true;

      chart.responsive.enabled = true;
      chart.responsive.useDefault = true;
      chart.innerRadius = am4core.percent(40);
    },
  }
};
</script>
