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
        :fill="isPitchClassActive(index) ? activeColor : 'white'"
        :stroke="isPitchClassActive(index) ? activeColor : 'black'"
        stroke-width="3"
      />

      <!-- Add the polygon representing the active pitch classes -->
      <polygon
        v-if="activePitchClassSet.length > 1"
        :points="getPolygonPoints()"
        :fill="activeColorWithOpacity"
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
        :fill="isPitchClassActive(index) ? 'white' : 'black'"
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
    activeColor: {
      type: String,
      default: "#007bff"
    },
    activeOpacity: {
      type: Number,
      default: 0.5,
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
    activeColorWithOpacity() {
      // Apply opacity to the active color
      const rgbaColor = this.hexToRgba(this.activeColor, this.activeOpacity);
      return rgbaColor;
    },
  },
  methods: {
    isPitchClassActive(index) {
      console.log("Checking pitch class:", index);
      return this.activePitchClassSet.includes(index);
    },
    getPolygonPoints() {
      // Calculate the points for the polygon representing the active pitch classes
      return this.activePitchClassSet
        .map((index) => {
          const angle = this.pitchClassAngles[index];
          const x = 250 - (this.circleSize / 2) * Math.cos(angle + Math.PI / 2);
          const y = 250 - (this.circleSize / 2) * Math.sin(angle + Math.PI / 2);
          return `${x},${y}`;
        })
        .join(" ");
    },
    hexToRgba(hexColor, opacity) {
      const hex = hexColor.replace("#", "");
      const r = parseInt(hex.substring(0, 2), 16);
      const g = parseInt(hex.substring(2, 4), 16);
      const b = parseInt(hex.substring(4, 6), 16);
      return `rgba(${r}, ${g}, ${b}, ${opacity})`;
    },
  },
  watch: {
    activePitchClassSet: {
      immediate: true, // Run the watcher immediately after the component is created
      handler(newActivePitchClassSet) {
        // Update the active pitch classes when the activePitchClassSet prop changes
        console.log("Updating active pitch classes:", newActivePitchClassSet);
      },
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
