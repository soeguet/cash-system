<template>
  <div class="card">
    <div class="reset">
      <button class="resetBtn button" @click="reset">reset</button>
    </div>
    <h1 class="header total">Cash System</h1>
    <main>
      <div class="container">
        <CustomNotes
          v-for="(value, field) in notes"
          :key="field"
          :field="field"
          :value="value"
          @updateNotes="updateNotes"
        />
      </div>
      <div class="totalRow notesRow">
        Total notes: {{ totalNotes.toFixed(2) }} €
      </div>
      <div class="container">
        <CustomCoins
          v-for="(value, field) in coins"
          :key="field"
          :field="field"
          :value="value"
          @updateCoins="updateCoins"
        />
      </div>
      <div class="totalRow">Total coins: {{ totalCoins.toFixed(2) }} €</div>

      <div class="total">Total: {{ total.toFixed(2) }} €</div>
    </main>
  </div>
</template>

<script setup lang="ts">
import CustomNotes from "@/components/CustomNotes.vue";
import CustomCoins from "@/components/CustomCoins.vue";
import { computed, reactive } from "vue";

type ValuesType = Record<string, number>;

const notes: ValuesType = reactive({
  "200 €": 0,
  "100 €": 0,
  "50 €": 0,
  "20 €": 0,
  "10 €": 0,
  "5 €": 0,
});

const coins: ValuesType = reactive({
  "2 €": 0,
  "1 €": 0,
  "0.5 €": 0,
  "0.2 €": 0,
  "0.1 €": 0,
  "0.05 €": 0,
  "0.02 €": 0,
  "0.01 €": 0,
});

function reset() {
  for (let key in notes) {
    notes[key] = 0;
  }
  for (let key in coins) {
    coins[key] = 0;
  }
}

const updateNotes = (field: string, newValue: number) => {
  notes[field] = newValue;
};
const updateCoins = (field: string, newValue: number) => {
  coins[field] = newValue;
};

const totalNotes = computed(() => {
  return Object.entries(notes).reduce(
    (sum, [field, count]) => sum + parseFloat(field) * count,
    0
  );
});
const totalCoins = computed(() => {
  return Object.entries(coins).reduce(
    (sum, [field, count]) => sum + parseFloat(field) * count,
    0
  );
});

const total = computed(() => {
  return (
    Object.entries(notes).reduce(
      (sum, [field, count]) => sum + parseFloat(field) * count,
      0
    ) +
    Object.entries(coins).reduce(
      (sum, [field, count]) => sum + parseFloat(field) * count,
      0
    )
  );
});
</script>

<style scoped>
.card {
  margin: auto;
  margin-top: 2%;
  border-color: grey;
  border-style: dashed;
  min-width: 1500px;
  width: 60%;
  padding: 3rem;
  border-radius: 50px;
  border-width: 1px;
}
.container {
  margin: auto;
  display: flex;
  justify-content: space-around;
}
.total {
  margin-top: 2rem;
  text-align: center;
  font-size: 2rem;
  width: 100%;
}
.totalRow {
  margin-bottom: 3rem;
  margin-top: 3rem;
  text-align: center;
  font-size: 1.5rem;
  width: 100%;
}
.reset {
  display: flex;
  justify-content: right;
}
.button {
  font-size: large;
  background: transparent;
  border-style: none;
}
.button:hover {
  transition: 1s;
  color: #a8b9ff;
}
.header {
  margin-top: 2rem;
  margin-bottom: 2rem;
  font-size: 3rem;
}
main {
  margin-top: 1rem;
}
.notesRow {
  margin-top: 0.5rem;
}
</style>
