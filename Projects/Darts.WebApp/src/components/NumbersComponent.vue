<template>
  <div
    class="q-pa-md flex flex-col items-center grid grid-rows-[auto,auto] gap-1"
  >
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
        @click="acceptSelection"
      />
    </div>
    <!-- <div class="mt-4">
      <input
        v-model="state.selectedNumber"
        type="text"
        class="border p-2 rounded"
        readonly
      />
    </div> -->
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, computed, ComputedRef } from 'vue';

interface State {
  selectedNumber: number | null;
  numbers: number[];
  modifiersDisabled: boolean;
  modifierApplied: string;
}

interface SelectionResult {
  number: number;
  modifier: string;
  selectedNumber: number;
}

export default defineComponent({
  name: 'NumberGrid',
  setup() {
    const state: State = reactive({
      selectedNumber: null,
      numbers: [
        1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20,
        25,
      ],
      modifiersDisabled: false,
      modifierApplied: '',
    });

    const canDouble: ComputedRef<boolean> = computed(
      () =>
        state.selectedNumber !== null &&
        state.selectedNumber !== 0 &&
        !state.modifiersDisabled,
    );
    const canTriple: ComputedRef<boolean> = computed(
      () =>
        state.selectedNumber !== null &&
        state.selectedNumber !== 0 &&
        state.selectedNumber !== 25 &&
        !state.modifiersDisabled,
    );

    const selectNumber = (num: number): void => {
      state.selectedNumber = num;
    };

    const applyModifier = (modifier: string): void => {
      if (state.selectedNumber !== null) {
        if (modifier === 'Double') {
          state.selectedNumber *= 2;
          state.modifierApplied = 'Double';
        } else if (modifier === 'Triple') {
          state.selectedNumber *= 3;
          state.modifierApplied = 'Triple';
        }
        state.modifiersDisabled = true;
      }
    };

    const deleteLast = (): void => {
      state.selectedNumber = null;
      state.modifiersDisabled = false;
    };

    const acceptSelection = (): SelectionResult => {
      if (state.selectedNumber === null) {
        return { number: 0, modifier: '', selectedNumber: 0 };
      }

      return {
        number: state.selectedNumber,
        modifier: state.modifierApplied,
        selectedNumber: state.selectedNumber,
      };
    };

    const isDisabled = (num: number): boolean => {
      if (num === 0 || (num === 25 && state.selectedNumber === 25)) {
        return true;
      }
      return false;
    };

    return {
      state,
      numbers: state.numbers,
      canDouble,
      canTriple,
      selectNumber,
      applyModifier,
      deleteLast,
      isDisabled,
      acceptSelection,
    };
  },
});
</script>
