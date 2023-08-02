<template>
  <div>
    <h2 class="mb-4">Pitch-Class Set Analysis</h2>
    <div class="form-group">
      <label for="pcSetInput">Enter Pitch-Class Set:</label>
      <input
        id="pcSetInput"
        class="form-control"
        type="text"
        v-model="pcSet"
        @input="analyze"
        placeholder="Enter pitch class set, e.g., 047 or 26E"
      />
    </div>
    <div v-if="analysisResult" class="mt-4">
      <p><strong>Normal Form:</strong> {{ analysisResult.normalForm }}</p>
      <p><strong>Prime Form:</strong> {{ analysisResult.primeForm }}</p>
      <p><strong>Forte Number:</strong> {{ analysisResult.forteNumber }}</p>
      <p><strong>Set Class:</strong> {{ analysisResult.setClass }}</p>
      <p><strong>Number of Symmetries:</strong> {{ analysisResult.numSymmetries }}</p>
      <p><strong>Cardinality:</strong> {{ analysisResult.cardinality }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pcSet: '',
      analysisResult: null,
    };
  },
  methods: {
    analyze() {
      const notes = this.pcSet.trim().toUpperCase();
      const pitchClasses = notes.split('').map((note) => {
        if (note === 'T') return 10;
        if (note === 'E') return 11;
        return parseInt(note);
      });

      // Sort the pitch classes in ascending order
      pitchClasses.sort((a, b) => a - b);

      // Calculate the normal form (transposition to the lowest pitch class)
      const transposition = pitchClasses[0];
      const normalForm = pitchClasses.map((pc) => (pc - transposition + 12) % 12);

      // Calculate the prime form (rotation to the most compact form)
      const primeForm = [...normalForm].sort((a, b) => a - b);

      // Calculate the Forte number (unique identifier for set classes)
      const forteNumber = primeForm.join('');

      // Calculate the set class (removing duplicate forms)
      const setClass = Array.from(new Set(primeForm)).join('');

      // Calculate the number of symmetries
      const numSymmetries = primeForm.length === 1 ? 1 : 12 / primeForm.length;

      // Calculate the cardinality (number of pitch classes in the set)
      const cardinality = primeForm.length;

      // Set the analysis results
      this.analysisResult = {
        normalForm: normalForm.join(''),
        primeForm: primeForm.join(''),
        forteNumber,
        setClass,
        numSymmetries,
        cardinality,
      };
    },
  },
};
</script>

<style>
/* No custom styles needed at this point */
</style>
