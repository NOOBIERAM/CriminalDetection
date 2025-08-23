<template>
  <div class="container mt-5">
    <div class="card p-4 shadow-lg">
      <h2 class="text-center mb-4">Criminal Detection System</h2>

      <!-- Sélection du suspect -->
      <div class="mb-3">
        <label class="form-label">Choose a suspect:</label>
        <select class="form-select" v-model="selectedSuspect" @change="hideProofs">
          <option v-for="suspect in suspects" :key="suspect" :value="suspect">{{ suspect }}</option>
        </select>
      </div>

      <!-- Sélection du crime -->
      <div class="mb-3">
        <label class="form-label">Choose a crime type:</label>
        <select class="form-select" v-model="selectedCrime" @change="hideProofs">
          <option v-for="crime in crimeTypes" :key="crime" :value="crime">{{ crime }}</option>
        </select>
      </div>

      <!-- Bouton -->
      <button class="btn btn-primary w-100" @click="checkGuilt">Check Guilt</button>

      <!-- Résultat -->
      <div v-if="showProofs" class="alert mt-3" :class="result ? 'alert-danger' : 'alert-success'">
        {{ result ? "Guilty" : "Not Guilty" }}
      </div>

      <!-- Affichage des preuves -->
      <div v-if="showProofs" class="mt-4">
        <h4>Proofs / Indices for {{ clickedSuspect }} ({{ clickedCrime }})</h4>
        <ul>
          <li v-if="clickedCrime === 'vol'">
            Motive: {{ facts.motives[clickedSuspect]?.includes('vol') ? 'Yes' : 'No' }}
          </li>
          <li v-if="clickedCrime === 'vol'">
            Near crime scene: {{ facts.nearScene[clickedSuspect]?.includes('vol') ? 'Yes' : 'No' }}
          </li>
          <li v-if="clickedCrime === 'vol'">
            Fingerprints on weapon: {{ facts.fingerprints[clickedSuspect]?.includes('vol') ? 'Yes' : 'No' }}
          </li>

          <li v-if="clickedCrime === 'assassinat'">
            Motive: {{ facts.motives[clickedSuspect]?.includes('assassinat') ? 'Yes' : 'No' }}
          </li>
          <li v-if="clickedCrime === 'assassinat'">
            Near crime scene: {{ facts.nearScene[clickedSuspect]?.includes('assassinat') ? 'Yes' : 'No' }}
          </li>
          <li v-if="clickedCrime === 'assassinat'">
            Fingerprints on weapon: {{ facts.fingerprints[clickedSuspect]?.includes('assassinat') ? 'Yes' : 'No' }}
          </li>

          <li v-if="clickedCrime === 'escroquerie'">
            Motive: {{ facts.motives[clickedSuspect]?.includes('escroquerie') ? 'Yes' : 'No' }}
          </li>
          <li v-if="clickedCrime === 'escroquerie'">
            Bank transactions: {{ facts.bankTransactions[clickedSuspect]?.includes('escroquerie') ? 'Yes' : 'No' }}
          </li>
          <li v-if="clickedCrime === 'escroquerie'">
            Fake identity: {{ facts.fakeIdentity[clickedSuspect]?.includes('escroquerie') ? 'Yes' : 'No' }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      suspects: ["john", "mary", "alice", "bruno", "sophie"],
      crimeTypes: ["vol", "assassinat", "escroquerie"],
      selectedSuspect: "john",
      selectedCrime: "vol",
      clickedSuspect: null,
      clickedCrime: null,
      result: null,
      showProofs: false,
      facts: {
        motives: {
          john: ["vol"],
          mary: ["assassinat"],
          alice: ["escroquerie"],
        },
        nearScene: {
          john: ["vol"],
          mary: ["assassinat"],
        },
        fingerprints: {
          john: ["vol"],
          mary: ["assassinat"],
        },
        bankTransactions: {
          alice: ["escroquerie"],
          bruno: ["escroquerie"],
        },
        fakeIdentity: {
          sophie: ["escroquerie"],
        },
      },
    };
  },
  methods: {
    hideProofs() {
      this.showProofs = false;
    },
    checkGuilt() {
      this.result = this.isGuilty(this.selectedSuspect, this.selectedCrime);
      this.clickedSuspect = this.selectedSuspect;
      this.clickedCrime = this.selectedCrime;
      this.showProofs = true;
    },
    isGuilty(suspect, crime) {
      let guilty = false;

      if (crime === "vol") {
        guilty =
          this.facts.motives[suspect]?.includes(crime) &&
          this.facts.nearScene[suspect]?.includes(crime) &&
          this.facts.fingerprints[suspect]?.includes(crime);
      } else if (crime === "assassinat") {
        guilty =
          this.facts.motives[suspect]?.includes(crime) &&
          this.facts.nearScene[suspect]?.includes(crime) &&
          (this.facts.fingerprints[suspect]?.includes(crime) || false);
      } else if (crime === "escroquerie") {
        guilty =
          (this.facts.bankTransactions[suspect]?.includes(crime) ||
            this.facts.fakeIdentity[suspect]?.includes(crime)) &&
          this.facts.motives[suspect]?.includes(crime);
      }

      return guilty;
    },
  },
};
</script>

<style>
body {
  background: #f8f9fa;
}
.card {
  border-radius: 15px;
}
</style>
