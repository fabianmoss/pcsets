<template>
  <div class="pc-set-visualizer">
    <svg :width="circleSize" :height="circleSize" viewBox="0 0 500 500">
      <!-- Add the pitch-class circle -->
      <circle
        cx="250"
        cy="250"
        :r="circleSize / 2"
        stroke="black"
        stroke-width="3"
        fill="none"
      />

      <!-- Add the smaller circles representing each pitch class -->
      <circle
        v-for="(angle, index) in pitchClassAngles"
        :key="index"
        :cx="250 - (circleSize / 2) * Math.cos(angle + Math.PI/2)"
        :cy="250 - (circleSize / 2) * Math.sin(angle + Math.PI/2)"
        r="20"
        :fill="isPitchClassActive(index) ? 'blue' : 'white'"
        :stroke="isPitchClassActive(index) ? 'blue' : 'black'"
        stroke-width="3"
      />

      <!-- Add the labels for pitch class numbers -->
      <text
        v-for="(label, index) in pitchClassLabels"
        :key="index"
        :x="250 - (circleSize / 2) * Math.cos(label.angle + Math.PI/2)"
        :y="250 - (circleSize / 2) * Math.sin(label.angle + Math.PI/2)"
        text-anchor="middle"
        font-size="20"
        alignment-baseline="central"
        font-weight="bold"
        :stroke="isPitchClassActive(index) ? 'white' : 'black'"
      >
        {{ label.label }}
      </text>
    </svg>
  </div>
</template>

<script>
export default {
  props: {
    activePitchClassSet: {
      type: Array,
      default: () => [],
    },
    parsePcSet: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      circleSize: 400, // Adjust this value to set the size of the pitch-class circle
    };
  },
  computed: {
    pitchClassAngles() {
      // Calculate the angles for each pitch class in radians
      return Array.from({ length: 12 }, (_, index) => (2 * Math.PI * index) / 12);
    },
    pitchClassLabels() {
      // Calculate the labels for pitch class numbers
      return Array.from({ length: 12 }, (_, index) => {
        const angle = this.pitchClassAngles[index];
        // const x = 50 + (this.circleSize / 2 - 15) * Math.cos(angle); // Adjust the position of the labels
        return { angle, label: index.toString() };
      });
    },
  },
  methods: {
    isPitchClassActive(index) {
      // Check if the pitch class at the specified index is in the active pitch-class set
      return this.activePitchClassSet.includes(index);
    },
  },
};
</script>

<style>
.pc-set-visualizer {
  margin-top: 1rem;
  display: flex;
  justify-content: center;
}
</style>
