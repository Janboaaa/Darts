<template>
  <div
    class="q-pa-md flex flex-col items-center grid grid-rows-[auto,auto] gap-1"
  >
    <!-- Zahlen-Buttons -->
    <div class="grid grid-cols-7 gap-1">
      <q-btn
        v-for="num in numbers"
        :key="num"
        :label="num"
        color="primary"
        class="flex items-center justify-center"
        :disable="isDisabled(num)"
        @click="selectNumber(num)"
      />
    </div>

    <!-- Spezialbuttons (0, Double, Triple, Delete) -->
    <div class="grid grid-cols-7 gap-1 place-content-start">
      <q-btn
        label="0"
        color="primary"
        class="col-span-1 flex items-center justify-center"
        @click="selectNumber(0)"
      />
      <q-btn
        label="Double"
        color="secondary"
        class="col-span-2 flex items-center justify-center"
        :disable="!canDouble"
        @click="applyModifier('Double')"
      />
      <q-btn
        label="Triple"
        color="secondary"
        class="col-span-2 flex items-center justify-center"
        :disable="!canTriple"
        @click="applyModifier('Triple')"
      />
      <q-btn
        label="Delete"
        color="negative"
        class="col-span-1 flex items-center justify-center"
        @click="deleteLast"
      />
      <q-btn
        label="Accept"
        color="positive"
        class="col-span-1 flex items-center justify-center"
        @click="deleteLast"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, computed } from 'vue';

export default defineComponent({
  name: 'NumberGrid',
  setup() {
    const state = reactive({
      selectedNumber: null as number | null,
      numbers: [
        1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20,
        25,
      ],
    });

    const canDouble = computed(
      () => state.selectedNumber !== null && state.selectedNumber !== 0,
    );

    const canTriple = computed(
      () =>
        state.selectedNumber !== null &&
        state.selectedNumber !== 0 &&
        state.selectedNumber !== 25,
    );

    const selectNumber = (num: number): void => {
      state.selectedNumber = num;
      console.log(`Selected number: ${num}`);
    };

    const applyModifier = (modifier: string): void => {
      console.log(`Applied modifier: ${modifier} to ${state.selectedNumber}`);
    };

    const deleteLast = (): void => {
      state.selectedNumber = null;
      console.log('Last selection deleted');
    };

    const isDisabled = (num: number): boolean => {
      if (num === 0 || (num === 25 && state.selectedNumber === 25)) {
        return true;
      }
      return false;
    };

    return {
      numbers: state.numbers,
      canDouble,
      canTriple,
      selectNumber,
      applyModifier,
      deleteLast,
      isDisabled,
    };
  },
});
</script>
