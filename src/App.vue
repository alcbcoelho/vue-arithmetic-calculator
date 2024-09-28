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
  <div class="main-content container p-4">
    <div class="main-content__heading">
      <h1 class="main-content__heading__title">Calculadora Aritmética</h1>
      <span class="main-content__heading__subtitle">
        <span>
          Desenvolvida com <a href="https://vuejs.org/" target="_blank">Vue.js</a>
        </span>
      </span>
    </div>
    <div class="d-flex flex-column gap-4">
      <div class="main-content__field-container w-25 m-auto">
        <label for="operation" class="form-label">Operação</label>
        <select id="operation" class="form-select" @change="onChangeSelect">
          <option value="+">Adição</option>
          <option value="-">Subtração</option>
          <option value="x">Multiplicação</option>
          <option value="÷">Divisão</option>
        </select>
      </div>
      <div class="position-relative">
        <div class="number-binding"></div>
        <div class="d-flex justify-content-center align-items-center gap-4">
          <div class="main-content__field-container w-25">
            <label for="number-1" class="form-label">Número 1</label>
            <input id="number-1" class="form-control" type="number" placeholder="x" @change="onChange">
          </div>
          <div class="main-content__field-container__operation">
            <span>
              {{ state.operation }}
            </span>
          </div>
          <div class="main-content__field-container w-25">
            <label for="number-2" class="form-label">Número 2</label>
            <input id="number-2" class="form-control" type="number" placeholder="y" @change="onChange">
          </div>
        </div>
      </div>
      <div v-if="(!Number.isNaN(state.result))" class="main-content__result">{{ state.result }}</div>
      <div v-else class="fw-bold">?</div>
    </div>
  </div>
  <footer class="container">
    <p class="fs-sm">&copy; 2024 - André Coêlho</p>
  </footer>
</template>

<style scoped>
.main-content {
  background-color: #ffffffcf;
  border-radius: 8px;
}

.main-content__heading {
  text-align: center;
  margin-bottom: 32px;
}

.main-content__heading__title,
.main-content__heading__subtitle {
  margin: 0;
}

.main-content__heading__subtitle {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-top: 8px;
  font-weight: 300;
  color: #213547bc;
}

.main-content__heading__subtitle::before,
.main-content__heading__subtitle::after {
  content: "";
  display: inline-block;
  width: 64px;
  height: 0.5px;
  background-color: #2135475f;
}

.main-content__field-container {
  position: relative;
}

.main-content__field-container__operation {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  border-radius: 8px;
  transform: rotate(45deg);
  color: #fff;
  background-color: #213547;
  font-weight: 500;
  z-index: 1;
}

.number-binding {
  position: absolute;
  top: 50%;
  left: 25%;
  height: 1px;
  width: 50%;
  background-color: #213547;
}

.main-content__field-container__operation span {
  transform: rotate(-45deg);
}

.main-content__result {
  position: relative;
  width: fit-content;
  margin: 0 auto;
  color: #2135475f;
  font-size: 4rem;
  font-weight: 800;
  text-align: center;
}

.main-content__result::before {
  position: absolute;
  top: 25%;
  left: -32px;
  font-size: 2rem;
  content: "= "
}
</style>
