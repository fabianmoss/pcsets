<template>
  <div id="app" class="container my-4">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <PcSetAnalysis />
      </div>
    </div>
  </div>
</template>

<script>
import PcSetAnalysis from './components/PcSetAnalysis.vue';

export default {
  components: {
    PcSetAnalysis,
  },
};
</script>

<style>
/* Add your styles here */
</style>
