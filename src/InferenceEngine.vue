<template>
  <div class="inference-engine">
    <div class="flex flex-row w-full">
      <fact-base ref="factBase"></fact-base>
      <production-base ref="productionBase"></production-base>
    </div>
    <div
      class="flex flex-col gap-4 p-4 justify-center text-center bg-stone-800"
    >
      <button
        class="bg-green-700 p-2 hover:bg-green-900 duration-150 w-fit"
        @click="runInference"
      >
        Запустити логіку виводу
      </button>
      <button
        class="bg-purple-700 hover:bg-purple-900 duration-150 p-2 w-fit"
        @click="clearResults"
      >
        Очистити результати діагностик
      </button>
    </div>

    <ul class="flex flex-col gap-2">
      <li
        class="bg-red-700 p-2 m-2 font-bold"
        v-for="(result, index) in results"
        :key="index"
      >
        {{ result }}
      </li>
    </ul>
  </div>
</template>

<script>
import FactBase from "./FactBase.vue";
import ProductionBase from "./ProductionBase.vue";

export default {
  data() {
    return {
      results: [],
    };
  },
  components: {
    FactBase,
    ProductionBase,
  },
  methods: {
    runInference() {
      for (const production of this.$refs.productionBase.productions) {
        if (this.evaluateCondition(production.condition)) {
          this.executeAction(production.action);
          console.log(production.action);
        }
      }
    },
    clearResults() {
      this.results = [];
    },

    evaluateCondition(condition) {
      return condition.split("&&").every((cond) => {
        cond = cond.trim();
        const isNegation = cond.startsWith("!");
        if (isNegation) cond = cond.substring(1);
        // Знайти факт у масиві, який відповідає назві факту `cond`
        const factObject = this.$refs.factBase.facts.find(
          (f) => f.fact === cond
        );
        if (!factObject) {
          console.error(`Факт ${cond} не знайдено`);
          return false;
        }
        const factValue = factObject.status;
        return isNegation ? !factValue : factValue;
      });
    },
    executeAction(action) {
      // Записуємо результати дії
      this.results.push(action);
    },
    // Приклад методу дії для діагностики
    diagnoseComponent(component) {
      // Логіка для визначення потенційної проблеми компонента та виведення рекомендацій
      const problems = {
        engine: "Перевірте рівень масла та охолоджувальної рідини.",
        battery: "Перевірте напругу акумулятора та стан з'єднань.",
        tire: "Перевірте тиск у шинах та наявність пошкоджень.",
      };
      if (problems[component]) {
        alert(`Проблема: ${component}\nДії: ${problems[component]}`);
      } else {
        alert(`Компонент '${component}' не визначено у базі даних проблем.`);
      }
    },
  },
};
</script>
