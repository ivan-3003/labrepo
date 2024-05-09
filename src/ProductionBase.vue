<template>
  <div class="production-base">
    <p>База продукцій:</p>
    <ul class="flex flex-col w-fit gap-8 m-4 justify-center">
      <li
        class="border-b-2 flex flex-col gap-4 border-gray-200"
        v-for="(rule, index) in productions"
        :key="index"
      >
        <strong class="text-green-700 bg-stone-800 p-2"
          >Продукція {{ index }}:</strong
        >
        <div class="flex flex-col gap-2">
          <p>Якщо</p>
          {{ rule.condition }}
          <p>Тоді</p>
          {{ rule.action }}
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      productions: [
        {
          condition: "pc_does_not_turn_on && pc_beep_codes",
          action: "Перевірте живлення жорсткого диска",
        },
        {
          condition: "car_engine_does_not_start && !car_noise",
          action: "Перевірте систему охолодження двигуна",
        },
        {
          condition: "pc_does_not_turn_on && car_noise && !pc_beep_codes",
          action: "Перевстановіть оперативну пам'ять",
        },
        {
          condition: "car_engine_does_not_start && car_noise",
          action: "Перевірте рівень охолоджувальної рідини",
        },
      ],
    };
  },
};
</script>
