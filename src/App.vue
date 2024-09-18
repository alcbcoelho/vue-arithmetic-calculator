<script setup>
import { reactive } from "vue";

const state = reactive({
  numbers: [0, 0],
  operation: "+",
  result: 0,
});

const onChange = function(e) {
  const index = (e.target.id === "number-1") ? 0 : 1;
  state.numbers[index] = +e.target.value;

  calculateResult();
}

const onChangeSelect = function(e) {
  state.operation = e.target.value;

  calculateResult();
}

const calculateResult = function() {
  const { numbers, operation } = state;
  let calculation = (acc, num) => acc + num;

  switch (operation) {
    case "+":
      calculation = (acc, num) => acc + num;
      break;
    case "-":
      calculation = (acc, num) => acc - num;
      break;
    case "x":
      calculation = (acc, num) => acc * num;
      break;
    case "÷":
      calculation = (acc, num) => acc / num;
  }

  state.result = numbers.reduce(calculation);
}
</script>

<template>
  <div class="container">
    <div>
      <select class="form-select w-25 m-auto" @change="onChangeSelect">
        <option value="+">Adição</option>
        <option value="-">Subtração</option>
        <option value="x">Multiplicação</option>
        <option value="÷">Divisão</option>
      </select>
    </div>
    <div class="d-flex justify-content-center align-items-center mt-4 gap-4">
      <input id="number-1" class="form-control w-25" type="number" placeholder="Insira um número" @change="onChange">
      <div>{{ state.operation }}</div>
      <input id="number-2" class="form-control w-25" type="number" placeholder="Insira outro número" @change="onChange">
      <div>=</div>
      <div v-if="(!Number.isNaN(state.result))" class="fw-bold">{{ state.result }}</div>
      <div v-else class="fw-bold">?</div>
    </div>
  </div>
</template>

<style scoped>
</style>
