<template>
  <div class="app">
    <card-form-background
      v-bind="{ cardholderName, cardNumber, cardMonth, cardYear, cardCvc }"
    />
    <card-form-active
      v-if="!submitted"
      @input-event="inputEventHandler"
      @submit-event="submitEventHandler"
    />
    <card-form-completed
      v-if="submitted"
      @reset-form-event="resetFormEventHandler"
    />
  </div>
</template>

<script setup>
import { ref } from "vue";
import CardFormBackground from "@/components/CardFormBackground.vue";
import CardFormActive from "@/components/CardFormActive.vue";
import CardFormCompleted from "@/components/CardFormCompleted.vue";

const cardholderName = ref();
const cardNumber = ref();
const cardMonth = ref();
const cardYear = ref();
const cardCvc = ref();
let submitted = ref(false);
let correctInputData = ref();

function inputEventHandler(
  nameValue,
  cardNumberValue,
  monthValue,
  yearValue,
  cvcValue
) {
  cardholderName.value = nameValue.value?.trim();
  cardNumber.value = cardNumberValue.value;
  cardMonth.value = monthValue.value;
  cardYear.value = yearValue.value;
  cardCvc.value = cvcValue.value;
}

function submitEventHandler(val1, val2, val3, val4, val5) {
  correctInputData.value = [
    val1.value,
    val2.value,
    val3.value,
    val4.value,
    val5.value,
  ];

  correctInputData.value.some((el) => el === false)
    ? (submitted.value = false)
    : (submitted.value = true);
}

function resetFormEventHandler() {
  cardholderName.value = "";
  cardNumber.value = "";
  cardMonth.value = "";
  cardYear.value = "";
  cardCvc.value = "";
  submitted.value = false;
}
</script>

<style lang="scss">
#app > div {
  display: flex;
}

.app {
  display: flex;
  flex-direction: row;
}

@media (max-width: 500px) {
  .app {
    display: flex;
    flex-direction: column;
  }
}
</style>
