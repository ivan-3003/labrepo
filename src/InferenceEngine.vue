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

/**
 * Компонент `InferenceEngine` відповідає за виконання логічного виводу на основі заданих фактів та продукцій.
 * Він використовує компоненти `FactBase` та `ProductionBase` для отримання даних та управління логікою виводу.
 *
 * @component
 */
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
    /**
     * Виконує процес логічного виводу, оцінюючи умови продукцій та виконуючи відповідні дії.
     */
    runInference() {
      for (const production of this.$refs.productionBase.productions) {
        if (this.evaluateCondition(production.condition)) {
          this.executeAction(production.action);
          console.log(production.action);
        }
      }
    },
    /**
     * Очищує всі результати діагностики.
     */
    clearResults() {
      this.results = [];
    },
    /**
     * Оцінює умову на основі наявних фактів.
     * @param {string} condition - Логічна умова для перевірки.
     * @returns {boolean}
     */
    evaluateCondition(condition) {
      return condition.split("&&").every((cond) => {
        cond = cond.trim();
        const isNegation = cond.startsWith("!");
        if (isNegation) cond = cond.substring(1);
        const factObject = this.$refs.factBase.facts.find(
          (f) => f.fact === cond
        );
        if (!factObject) {
          console.error(`Факт ${cond} не знайдено`);
          return false;
        }
        return isNegation ? !factObject.status : factObject.status;
      });
    },
    /**
     * Записує дії, виконані на основі продукцій.
     * @param {string} action - Дія для виконання.
     */
    executeAction(action) {
      this.results.push(action);
    },
  },
};
</script>

<docs lang="md">
Компонент `InferenceEngine` використовується для управління процесом логічного виводу в системі діагностики. Цей процес включає оцінку умов і виконання дій на основі заданих фактів та продукцій.

## Опис

Компонент забезпечує інтерфейс для взаємодії з базами фактів (`FactBase`) та продукцій (`ProductionBase`), дозволяючи користувачам запускати та очищати діагностику системи.

## Приклади використання

### Запуск логічного виводу

Використовуючи компонент для запуску процесу діагностики:

```html
<inference-engine></inference-engine>
```
</docs>
