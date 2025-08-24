<template>
  <div class="container mt-5">
    <div class="card p-4 border-0 rounded shadow-lg">
      <h2 class="text-center mb-4">Criminal Detection System</h2>
      <hr>
      </hr>
      <div class="row">
        <div class="col">
          <!-- Sélection du suspect -->
          <div class="mb-3">
            <label class="form-label">Choose a suspect:</label>
            <select class="form-select" v-model="selectedSuspect" @change="hideProofs">
              <option v-for="suspect in suspect" :key="suspect" :value="suspect">{{ suspect }}</option>
            </select>
          </div>

          <!-- Sélection du crime -->
          <div class="mb-3">
            <label class="form-label">Choose a crime type:</label>
            <select class="form-select" v-model="selectedCrime" @change="hideProofs">
              <option v-for="crime in crime_types" :key="crime" :value="crime">{{ crime }}</option>
            </select>
          </div>
          <button class="btn btn-success w-100" @click="checkGuilt">Check Guilt</button>

          <!-- Résultat -->
          <div v-if="showProofs" class="alert mt-3" :class="result ? 'alert-danger' : 'alert-success'">
            <strong>Result:</strong> {{ result ? "Guilty" : "Not Guilty" }}
          </div>

          <!-- Affichage des preuves -->
          <div v-if="showProofs" class="mt-4">
            <h5>Proofs / Indices for {{ clickedSuspect }} ({{ clickedCrime }})</h5>
            <ul>
              <li v-if="clickedCrime === 'vol'">
                Motive: {{ facts.has_motive[clickedSuspect]?.includes('vol') ? 'Yes' : 'No' }}
              </li>
              <li v-if="clickedCrime === 'vol'">
                Near crime scene: {{ facts.was_near_crime_scene[clickedSuspect]?.includes('vol') ? 'Yes' : 'No' }}
              </li>
              <li v-if="clickedCrime === 'vol'">
                Fingerprints on weapon: {{ facts.has_fingerprint_on_weapon[clickedSuspect]?.includes('vol') ? 'Yes' :
                  'No'
                }}
              </li>

              <li v-if="clickedCrime === 'assassinat'">
                Motive: {{ facts.has_motive[clickedSuspect]?.includes('assassinat') ? 'Yes' : 'No' }}
              </li>
              <li v-if="clickedCrime === 'assassinat'">
                Near crime scene: {{ facts.was_near_crime_scene[clickedSuspect]?.includes('assassinat') ? 'Yes' : 'No'
                }}
              </li>
              <li v-if="clickedCrime === 'assassinat'">
                Fingerprints on weapon: {{ facts.has_fingerprint_on_weapon[clickedSuspect]?.includes('assassinat') ?
                  'Yes' :
                  'No' }}
              </li>

              <li v-if="clickedCrime === 'escroquerie'">
                Motive: {{ facts.has_motive[clickedSuspect]?.includes('escroquerie') ? 'Yes' : 'No' }}
              </li>
              <li v-if="clickedCrime === 'escroquerie'">
                Bank transactions: {{ facts.has_bank_transaction[clickedSuspect]?.includes('escroquerie') ? 'Yes' : 'No'
                }}
              </li>
              <li v-if="clickedCrime === 'escroquerie'">
                Fake identity: {{ facts.owns_fake_identity[clickedSuspect]?.includes('escroquerie') ? 'Yes' : 'No' }}
              </li>
            </ul>
          </div>
        </div>
        <div class="col">
          <div class="bg-light rounded mb-3 p-3">
            <div>
            <h5>Rules of Culpability (Notes)</h5>
            <ul>
              <li><strong>Vol:</strong> Mobile + Presence + Fingerprint</li>
              <li><strong>Assassinat:</strong> Mobile + Presence + (Fingerprint or Eyewitness)</li>
              <li><strong>Escroquerie:</strong> Mobile + (Bank Transaction or Fake Identity)</li>
            </ul>
          </div>
          </div>
          <div class="bg-light p-3 rounded">
            <div>
            <h5>Group member (M1 GB Group 2)</h5>
            <ul>
              <li><span class="ms-2 strong me-3">2568</span> RAZAFINANDRIANINS Rios</li>
              <li><span class="ms-2 strong me-3">2585</span> RASOLOMANDIMBY Tendry Antenaina Steeve</li>
              <li><span class="ms-2 strong me-3">2591</span> RAZAKANTOANINA ANDRIAMAMONJISOA Brandon</li>
              <li><span class="ms-2 strong me-3">2613</span> RAKOTONANDRIANINA Dimithry Marius</li>
              <li><span class="ms-2 strong me-3">2691</span> RAMAMNATSOA Rotsiniaina</li>
              <li><span class="ms-2 strong me-3">2741</span> RICHARTS Diary Fenohasina Ravalo</li>
              <li><span class="ms-2 strong me-3">2761</span> RAKOTONDRAZANDRY Kenny Urvano</li>
              <li><span class="ms-2 strong me-3">2766</span> ANDRIAMAHAFALY Tsilavina Henintsoa</li>
            </ul>
          </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      suspect: ["john", "mary", "alice", "bruno", "sophie"],
      crime_types: ["vol", "assassinat", "escroquerie"],
      selectedSuspect: "john",
      selectedCrime: "vol",
      clickedSuspect: null,
      clickedCrime: null,
      result: null,
      showProofs: false,
      facts: {
        has_motive: {
          john: ["vol"],
          mary: ["assassinat"],
          alice: ["escroquerie"],
        },
        was_near_crime_scene: {
          john: ["vol"],
          mary: ["assassinat"],
        },
        has_fingerprint_on_weapon: {
          john: ["vol"],
          mary: ["assassinat"],
        },
        has_bank_transaction: {
          alice: ["escroquerie"],
          bruno: ["escroquerie"],
        },
        owns_fake_identity: {
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
          this.facts.has_motive[suspect]?.includes(crime) &&
          this.facts.was_near_crime_scene[suspect]?.includes(crime) &&
          this.facts.has_fingerprint_on_weapon[suspect]?.includes(crime);
      } else if (crime === "assassinat") {
        guilty =
          this.facts.has_motive[suspect]?.includes(crime) &&
          this.facts.was_near_crime_scene[suspect]?.includes(crime) &&
          (this.facts.has_fingerprint_on_weapon[suspect]?.includes(crime) || false);
      } else if (crime === "escroquerie") {
        guilty =
          (this.facts.has_bank_transaction[suspect]?.includes(crime) ||
            this.facts.owns_fake_identity[suspect]?.includes(crime)) &&
          this.facts.has_motive[suspect]?.includes(crime);
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
