<script setup lang="ts">
import ChartMemberAccordingComponent from './components/ChartMemberAccordingComponent.vue';
import ChartSectionComponent from './components/ChartSectionComponent.vue';
import ChartPieSectionComponent from './components/ChartPieSectionComponent.vue';
import ChartMemberPercentageComponent from './components/ChartMemberPercentageComponent.vue';
import ChartMemberSectionComponent from './components/ChartMemberSectionComponent.vue';
</script>

<template>
  <div class="container-fluid">
    <h3>Dashboard</h3>
    <div class="row">
      <div class="col-md-4">
        <ChartPieSectionComponent />
      </div>
      <div class="col-md-4">
        <ChartSectionComponent />
      </div>
      <div class="col-md-4">
        <ChartMemberAccordingComponent />
      </div>
    </div>
    <div class="row">
      <div class="col-md-4">
        <ChartMemberSectionComponent />
      </div>
      <div class="col-md-8">
        <ChartMemberPercentageComponent />
      </div>
    </div>
  </div>
</template>
