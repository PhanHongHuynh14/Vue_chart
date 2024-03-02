<template>
  <h3>Member According To Projects</h3>
  <div class="card-body">
    <div id="donutChartId"></div>
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
    donutChartId: String
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
        const res = await axios.get('http://localhost:8000/api/cv_project', { withCredentials: true });
        this.chartData = res.data.data;
        this.createChart();
      } catch (error) {
        console.error('Error fetching data from API:', error);
      }
    },

    createChart() {
      const sortedData = this.chartData.slice(0).sort((a, b) => b.value - a.value);
      const uniqueSections = [...new Set(this.chartData.slice(0,40).map(cv_project => cv_project.project_name))];
      const sectionData = [];

      uniqueSections.forEach(project => {
        const filteredData = this.chartData.filter(cv_project => cv_project.project_name === project);
        sectionData.push({
          category: project,
          value: filteredData.length,
        });
      });

      const chart = am4core.create("donutChartId", am4charts.PieChart);
      chart.data = sectionData;

      const pieSeries = chart.series.push(new am4charts.PieSeries());
      pieSeries.dataFields.value = "value";
      pieSeries.dataFields.category = "category";
      pieSeries.slices.template.fill = am4core.colorSet;

      chart.responsive.enabled = true;
      chart.responsive.useDefault = false;
      chart.innerRadius = am4core.percent(40);
    },
  }
};
</script>
  