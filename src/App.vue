<template>
  <div class="app">
    <!-- v-bind="{ cardholderName, cardNumber, cardMonth, cardYear, cardCvc }" -->
    <card-form-background :form="formState" />
    <card-form-active
      v-if="!submitted"
      @input-event="inputEventHandler"
      @submit-event="submitEventHandler"
    />
    <card-form-completed v-else @reset-form-event="resetFormEventHandler" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import CardFormBackground from "@/components/CardFormBackground.vue";
import CardFormActive from "@/components/CardFormActive.vue";
import CardFormCompleted from "@/components/CardFormCompleted.vue";

let formState = ref({});

let submitted = ref(false);

function inputEventHandler(form) {
  Object.assign(formState.value, form);
}

function submitEventHandler(form) {
  submitted.value = !Object.values(form).some((obj) => obj.error === true);
}

function resetFormEventHandler() {
  submitted.value = false;

  for (let i = 0; i < Object.keys(formState.value).length; i++) {
    formState.value[Object.keys(formState.value)[i]].value = "";
  }
}
</script>

<style lang="scss">
#app > div {
  display: flex;
}

.app {
  display: flex;
  flex-direction: column;
  justify-content: stretch;
}

@media (min-width: 1300px) {
  .app {
    display: flex;
    flex-direction: row;
  }
}
</style>
