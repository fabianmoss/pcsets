<template>
  <div>
    <h2 class="mb-4">Pitch-Class Set Analysis</h2>
    <div class="alert alert-warning" role="alert">
      This web app is under development. Some features may not work as expected.
    </div>
    <div class="form-group position-relative">
      <label for="pcSetInput">Enter Pitch-Class Set:</label>
      <input
        id="pcSetInput"
        class="form-control"
        type="text"
        v-model.trim="pcSet"
        @input="analyze(pcSet)"
        @keydown.enter="logSet(pcSet)"
        placeholder="Enter pitch class set, e.g., 047 or 26E"
        title="You entered invalid characters for pitch classes. Only 0-9, T, and E are valid."
        :class="{
          'is-invalid': invalidPitchClass
        }"
      />
      <div class="invalid-feedback" v-if="invalidPitchClass">
        Please enter valid pitch classes (0-9, T, or E).
      </div>
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
          @mouseover="showRemoveButton = set"
          @mouseout="showRemoveButton = null"
        >
          {{ set }}
          <span class="remove-button" @click="removeSet(set)">
            <i class="fas fa-times"></i> <!-- Font Awesome "times" icon -->
          </span>
        </button>
      </div>
    </div>
    <div v-if="analysisResult" class="mt-4">
      <!-- <p><strong>Normal Form:</strong> {{ analysisResult.normalForm }}</p>
      <p><strong>Prime Form:</strong> {{ analysisResult.primeForm }}</p>
      <p><strong>Forte Number:</strong> {{ analysisResult.forteNumber }}</p>
      <p><strong>Set Class:</strong> {{ analysisResult.setClass }}</p>
      <p><strong>Number of Symmetries:</strong> {{ analysisResult.numSymmetries }}</p> -->
      <p><strong>Cardinality:</strong> {{ analysisResult.cardinality }}</p>
    </div>
    <!-- Add the PcSetVisualizer component below the analytical output -->
    <PcSetVisualizer 
      :activePitchClassSet="parsePcSet(selectedSet)" 
      :active-color="activeColor"
      :parsePcSet="parsePcSet"
      :key="selectedSet"
      @removeSet="removeSet"
    />
  </div>
</template>

<script>
import PcSetVisualizer from "./PcSetVisualizer.vue";
export default {
  data() {
    return {
      pcSet: "",
      analysisResult: null,
      loggedSets: [],
      selectedSet: "",
      activeColor: "#007bff",
      showRemoveButton: null,
    };
  },
  computed: {
    parsedPcSet() {
      return this.parsePcSet(this.pcSet);
    },
    invalidPitchClass() {
      return this.parsedPcSet.some((pc) => pc < 0 || pc > 11);
    },
  },
  watch: {
    loggedSets: {
      handler(newLoggedSets) {
        if (!newLoggedSets.includes(this.selectedSet)) {
          this.selectedSet = ""; // Reset selectedSet if it's not present in loggedSets
        }
      },
      deep: true,
    },
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
      if (this.invalidPitchClass || this.pcSet === "") {
        // If the input is invalid or empty, don't log the set and show an error message
        return;
      }

      const enteredSet = this.pcSet.trim().toUpperCase();
      
      // Convert multiset to set
      const set = Array.from(new Set(enteredSet)).sort().join('');

      if (set !== "" && !this.loggedSets.includes(set)) {
        this.loggedSets.push(set);
      }
      this.pcSet = "";
      this.selectedSet = set;
    },
    // Add the parsePcSet method
    parsePcSet(set) {
      const parsed = set
        .toUpperCase()
        .split('')
        .map((item) => (item === 'T' ? 10 : item === 'E' ? 11 : Number(item)))
        .sort()
      return parsed;
    },
    selectSet(set) {
      this.selectedSet = set;
      this.analyze(set); // Call the analyze method when a logged set is selected
    },
    clearSelectedSet() {
      this.selectedSet = ""; // Set selectedSet to an empty string to deactivate all pitch classes
    },
    removeSet(set) {
      const index = this.loggedSets.indexOf(set);
      if (index !== -1) {
        this.loggedSets.splice(index, 1);
      }
      if (this.selectedSet === set) {
        this.clearSelectedSet(); // Call clearSelectedSet if the selected set is being removed
      }
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
    removeLoggedSet(set) {
      const index = this.loggedSets.indexOf(set);
      if (index !== -1) {
        this.loggedSets.splice(index, 1);
        if (set === this.selectedSet) {
          this.selectedSet = "";
          this.analysisResult = null;
        }
      }
    },
  },
  components: {
    PcSetVisualizer,
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
  position: relative; /* Add this line to position the remove button properly */
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

.pcset-buttons {
  display: flex;
  gap: 5px; /* Add some space between the buttons (you can adjust this value) */
}

.remove-button {
  position: absolute;
  top: 5px;
  right: 5px;
  font-weight: bold;
  cursor: pointer;
  opacity: 0;
  transition: opacity 0.2s ease-in-out;
}

.btn:hover .remove-button {
  opacity: 1;
}

/* Style for invalid input */
.is-invalid {
  border-color: red;
}

/* Style for invalid feedback message */
.invalid-feedback {
  color: red;
}
</style>
