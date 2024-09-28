<script setup>
import { reactive } from "vue";

const state = reactive({
  numbers: [undefined, undefined],
  operation: "+",
});

const onChange = function (e) {
  const index = e.target.id === "number-1" ? 0 : 1;
  state.numbers[index] = +e.target.value;

  calculateResult();
};

const onChangeSelect = function (e) {
  state.operation = e.target.value;

  calculateResult();
};

const calculateResult = function () {
  const { numbers, operation } = state;
  let calculation = (num1, num2) => num1 + num2;

  switch (operation) {
    case "+":
      calculation = (num1, num2) => num1 + num2;
      break;
    case "-":
      calculation = (num1, num2) => num1 - num2;
      break;
    case "x":
      calculation = (num1, num2) => num1 * num2;
      break;
    case "÷":
      calculation = (num1, num2) => num1 / num2;
  }

  return numbers.reduce(calculation);
};
</script>

<template>
  <div class="container px-0">
    <div class="main-content p-4">
      <div class="main-content__heading">
        <h1 class="main-content__heading__title">Calculadora Aritmética</h1>
        <span class="main-content__heading__subtitle">
          <span>
            Desenvolvida com
            <a href="https://vuejs.org/" target="_blank">Vue.js</a>
          </span>
        </span>
      </div>
      <div class="d-flex flex-column gap-4">
        <div class="main-content__field-container m-md-auto">
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
          <div
            class="d-md-flex justify-content-md-center align-items-md-center gap-4"
          >
            <div class="main-content__field-container">
              <label for="number-1" class="form-label">Número 1</label>
              <input
                id="number-1"
                class="form-control"
                type="number"
                placeholder="x"
                @change="onChange"
              />
            </div>
            <div class="main-content__field-container__operation">
              <span>
                {{ state.operation }}
              </span>
            </div>
            <div class="main-content__field-container">
              <label for="number-2" class="form-label">Número 2</label>
              <input
                id="number-2"
                class="form-control"
                type="number"
                placeholder="y"
                @change="onChange"
              />
            </div>
          </div>
        </div>
      </div>
      <div v-if="Number.isNaN(calculateResult())" class="main-content__result">
        ?
      </div>
      <div v-else class="main-content__result">
        {{
          Number.isInteger(calculateResult())
            ? calculateResult()
            : calculateResult().toFixed(4)
        }}
      </div>
    </div>
  </div>
  <footer class="container px-0">
    <p>&copy; 2024 - André Coêlho</p>
  </footer>
</template>

<style scoped>
.main-content {
  background-color: #ffffffcf;
  border-radius: 8px;
}

.main-content__heading {
  text-align: center;
  margin-bottom: 48px;
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
  margin-top: 16px;
  font-size: 0.75rem;
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
  width: 25%;
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
  display: block;
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
  margin: 32px auto 0 auto;
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
  content: "= ";
}

@media screen and (max-width: 767px) {
  .main-content__field-container {
    width: 100%;
  }

  .number-binding {
    display: block;
    position: absolute;
    top: 0%;
    left: calc(50% - 1px);
    height: 100%;
    width: 1px;
    background-color: #213547;
  }

  .main-content__heading {
    margin-bottom: 32px;
  }

  .main-content__heading__subtitle::before,
  .main-content__heading__subtitle::after {
    width: 12.5%;
  }

  .main-content__field-container__operation {
    margin: 16px auto;
  }

  .main-content__result {
    margin-top: 16px;
  }
}
</style>
