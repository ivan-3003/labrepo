<template>
  <div class="fact-base">
    <p>Фактична база:</p>
    <ul>
      <li
        class="flex flex-col gap-4 p-2 m-2 w-fit border border-gray-200 rounded-md"
        @click="updateFact(key, value)"
        v-for="(value, key) in facts"
        :key="key"
      >
        <div class="flex flex-row gap-12">
          <div class="flex flex-col gap-2 min-w-[30vw]">
            <h2>{{ value.description }}</h2>
            <span class="text-gray-400 text-sm">{{ value.fact }}</span>
          </div>
          <p
            class="min-w-12 cursor-pointer text-gray-100 font-bold items-center justify-center flex rounded-md p-2"
            :class="value.status ? 'bg-green-700' : 'bg-red-800'"
          >
            {{ value.status ? "Так" : "Ні" }}
          </p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Оновлена структура фактів
      facts: [
        {
          fact: "pc_does_not_turn_on",
          status: false,
          description: "Комп'ютер не вмикається",
        },
        {
          fact: "pc_reboots",
          status: false,
          description: "Комп'ютер перезавантажується",
        },
        {
          fact: "pc_beep_codes",
          status: true,
          description: "Звуки піп-кодів",
        },
        {
          fact: "car_engine_does_not_start",
          status: false,
          description: "Двигун не заводиться",
        },
        {
          fact: "car_engine_overheats",
          status: false,
          description: "Перегрів двигуна",
        },
        {
          fact: "car_noise",
          status: false,
          description: "Шум під час руху",
        },
      ],
      //pc_does_not_turn_on: false, // Комп'ютер не вмикається
      //pc_reboots: false, // Комп'ютер перезавантажується
      //pc_beep_codes: true, // Звуки піп-кодів
      //
      //car_engine_does_not_start: false, // Двигун не заводиться
      //car_engine_overheats: false, // Перегрів двигуна
      //car_noise: false, // Шум під час руху
      //},
    };
  },
  methods: {
    updateFact(index) {
      this.facts[index].status = !this.facts[index].status;
    },
  },
};
</script>
