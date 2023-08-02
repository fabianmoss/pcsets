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
        @input="analyze(pcSet)"
        @keydown.enter="logSet(pcSet)"
        placeholder="Enter pitch class set, e.g., 047 or 26E"
      />
    </div>
    <!-- Display the list of logged sets -->
    <div class="mt-4">
      <h3>Logged Sets:</h3>
      <div class="pcset-buttons">
        <button
          v-for="set in loggedSets"
          :key="set"
          @click="selectSet(set)"
          class="btn btn-secondary"
          :class="{ 'active': set === selectedSet }"
        >
          {{ set }}
        </button>
      </div>
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
      loggedSets: [],
      selectedSet: '',
    };
  },
  methods: {
    analyze(set) {
      const analysisResult = {
        normalForm: this.getNormalForm(set),
        primeForm: this.getPrimeForm(set),
        forteNumber: this.getForteNumber(set),
        setClass: this.getSetClass(set),
        numSymmetries: this.getNumSymmetries(set),
        cardinality: set.length,
      };
      this.analysisResult = analysisResult;
    },
    logSet() {
      const enteredSet = this.pcSet.trim().toUpperCase();
      if (enteredSet !== '' && !this.loggedSets.includes(enteredSet)) {
        this.loggedSets.push(enteredSet);
      }
      this.pcSet = '';
      this.selectedSet = enteredSet;
    },
    selectSet(set) {
      this.selectedSet = set;
      this.analyze(set); // Call the analyze method when a logged set is selected
    },
    // Your analysis functions here, working directly with the input string set
    getNormalForm(set) {
      // Placeholder example: Return the set itself as the normal form
      return set;
    },
    getPrimeForm(set) {
      // Placeholder example: Return the set itself as the prime form
      return set;
    },
    getForteNumber(set) {
      // Placeholder example: Return '0' as the Forte number
      return set;
    },
    getSetClass(set) {
      // Placeholder example: Return 'Unknown' as the set class
      return set;
    },
    getNumSymmetries(set) {
      // Placeholder example: Return '0' as the number of symmetries
      return set;
    },
  },
};
</script>

<style>
.btn-group {
  margin-top: 1rem;
}

.pcset-buttons {
  display: flex;
  gap: 5px; /* Add some space between the buttons (you can adjust this value) */
}

.btn {
  cursor: pointer;
  flex: 0 0 auto; /* Each button will only span the width of the content (pcset entered) */
}

.btn-secondary {
  background-color: #ccc;
  color: #fff;
  border: 1px solid #ccc;
  margin-right: 5px;
}

.btn.active {
  background-color: #007bff !important; /* Change this to your preferred active color */
  border-color: #007bff !important;
}

.btn:not(.active):hover {
  background-color: #eee; /* Change this to your preferred inactive hover color */
  border-color: #aaa; /* Change this to your preferred inactive border color */
  color: #000;
}
</style>
