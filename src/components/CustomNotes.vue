<script setup lang="ts">
import { ref, watch } from "vue";

const props = defineProps({
  field: String,
  value: Number,
});

const inputValue = ref(props.value || 0);

const multiplier = parseFloat(props.field as string);

const emit = defineEmits(["updateNotes"]);

watch(
  () => props.value,
  (newVal) => {
    inputValue.value = newVal as number;
  }
);
function increment() {
  inputValue.value++;
  emit("updateNotes", props.field, inputValue.value);
}

function decrement() {
  if (inputValue.value > 0) {
    inputValue.value--;
    emit("updateNotes", props.field, inputValue.value);
  }
}
function updateValue(event: any) {
  const spinnerNumber: number = parseInt(event.target.value);
  emit("updateNotes", props.field, spinnerNumber);
}
</script>

<template>
  <div class="main">
    <img :src="`${props.field}.png`" :alt="`${props.field}`" />
    <h1>{{ field }}</h1>
    <input type="number" :value="inputValue" @input="updateValue($event)" />
    <div class="control">
      <button type="button" class="button" @click="increment" tabindex="-1">
        +
      </button>
      <button type="button" class="button" @click="decrement" tabindex="-1">
        -
      </button>
    </div>
    <h3 class="total">= {{ ((value || 0) * multiplier).toFixed(2) }} €</h3>
  </div>
</template>

<style scoped>
.main {
  display: grid;
  text-align: center;
  align-content: center;
  margin: 2.5rem;
  padding: 1.5rem;
  height: 12rem;
  font-size: 18px;
}
.button {
  background-color: transparent;
  border-width: 1px;
  border-radius: 5px;
  border-style: groove;
  min-width: 30px;
  min-height: 30px;
  margin: 0.2rem;
}

.button:hover {
  transition: 1s;
  background-color: #a8b9ff;
}

.control {
  margin-top: 0.3rem;
}

.total {
  text-align: left;
  min-width: 100px;
  min-height: 30px;
  box-sizing: border-box;
  overflow: hidden;
  text-overflow: clip;
  white-space: nowrap;
}
input {
  margin: auto;
  width: 110px;
  max-width: 110px;
  box-sizing: border-box;
  font-size: 18px;
}

img {
  margin: auto;
  height: 45px;
}
</style>
