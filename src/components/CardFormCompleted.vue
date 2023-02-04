<template>
  <div class="card-form-completed">
    <div class="form-completed">
      <img
        class="card-form-completed__image-completed"
        src="../assets/images/icon-complete.svg"
        alt=""
      />

      <div class="form-completed__title">Thank you!</div>
      <div class="form-completed__subtitle">We've added your card details</div>
      <button
        @click="emit('resetFormEvent')"
        class="form-completed__btn-continue btn"
      >
        Continue
      </button>
    </div>
  </div>
</template>

<script setup>
import { defineEmits } from "vue";
const emit = defineEmits("resetFormEvent");
</script>

<style lang="scss" scoped>
.card-form-completed {
  display: flex;
  align-items: center;
  width: calc(100vw - 30%);
  justify-content: center;
}

.form-completed {
  width: 400px;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;

  &__title {
    font-size: 32px;
    margin: 20px 0 10px;
  }

  &__subtitle {
    margin: 10px 0 40px;
    opacity: 0.6;
  }
}
</style>
