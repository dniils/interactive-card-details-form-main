<template>
  <div class="card-form-active">
    <form @submit.prevent="submitForm" class="card-form">
      <div class="card-form__inputBox">
        <div class="card-form__title">Cardholder Name</div>
        <input
          @blur="validateCardholderName"
          @input="nameInputHandler"
          v-model="form.cardholderName.value"
          class="card-form__input"
          :class="{ wrong: form.cardholderName.error }"
          placeholder="Jane Appleseed"
          type="text"
          inputmode="text"
          required
        />
        <div v-if="form.cardholderName.error" class="card-form__error">
          Please, enter your Name and Surname
        </div>
      </div>

      <div class="card-form__inputBox">
        <div class="card-form__title">Card Number</div>
        <input
          @blur="validateCardNumber"
          @input="cardNumberInputHandler"
          v-model="form.cardNumber.value"
          class="card-form__input"
          :class="{ wrong: form.cardNumber.error }"
          type="text"
          placeholder="0000 0000 0000 0000"
          inputmode="numeric"
          required
        />
        <div v-if="form.cardNumber.error" class="card-form__error">
          Only 16 digits cards allowed
        </div>
      </div>

      <div class="card-form__flexbox">
        <div class="card-form__inputBox">
          <div class="card-form__title">Exp. Date</div>
          <div class="card-form__flexbox">
            <input
              @blur="validateMonth"
              @input="cardMonthInputHandler"
              v-model="form.cardMonth.value"
              class="card-form__input card-form__input_month-year"
              :class="{ wrong: form.cardMonth.error }"
              placeholder="MM"
              inputmode="numeric"
              required
            />

            <input
              @blur="validateYear"
              @input="cardYearInputHandler"
              v-model="form.cardYear.value"
              class="card-form__input card-form__input_month-year"
              :class="{ wrong: form.cardYear.error }"
              placeholder="YY"
              inputmode="numeric"
              required
            />
            <div
              v-if="form.cardMonth.error || form.cardYear.error"
              class="card-form__error"
            >
              Check format
            </div>
          </div>
        </div>

        <div class="card-form__inputBox">
          <div class="card-form__title">CVC</div>
          <input
            @blur="validateCvc"
            @input="cardCvcInputHandler"
            v-model="form.cardCvc.value"
            class="card-form__input card-form__input_cvc"
            :class="{ wrong: form.cardCvc.error }"
            placeholder="123"
            inputmode="numeric"
            required
          />
          <div v-if="form.cardCvc.error" class="card-form__error">
            3-digit number on the back
          </div>
        </div>
      </div>

      <button class="card-form__btn btn">Confirm</button>
    </form>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";

let form = ref({
  cardholderName: { value: "", error: false },
  cardNumber: { value: "", error: false },
  cardCvc: { value: "", error: false },
  cardMonth: { value: "", error: false },
  cardYear: { value: "", error: false },
});

const emit = defineEmits("inputEvent");

emitVariables();

function emitVariables() {
  emit("inputEvent", form.value);
}

function submitForm() {
  emit("submitEvent", form.value);
}

// Allow names like "Eugene Joseph-le-brun"
function nameInputHandler() {
  form.value.cardholderName.value = form.value.cardholderName.value
    .replace(/([^a-z ]+)/gi, "")
    .replace(/\s+/g, " ")
    .split(" ")
    .map((word) => {
      if (word) return word[0].toUpperCase() + word.substring(1).toLowerCase();
    })
    .join(" ");
}

function cardNumberInputHandler() {
  form.value.cardNumber.value = form.value.cardNumber.value
    .replace(/[^0-9]*/g, "")
    .slice(0, 16)
    .split("")
    .map((digit, i) => ((i + 1) % 4 === 1 ? " " + digit : digit))
    .join("")
    .slice(1);
}

function cardCvcInputHandler() {
  form.value.cardCvc.value = form.value.cardCvc.value
    .replace(/[^0-9]*/g, "")
    .slice(0, 3);
}

function cardMonthInputHandler() {
  form.value.cardMonth.value = form.value.cardMonth.value
    .replace(/[^0-9]*/g, "")
    .slice(0, 2);
}

function cardYearInputHandler() {
  form.value.cardYear.value = form.value.cardYear.value
    .replace(/[^0-9]/g, "")
    .slice(0, 2);
}

function validateYear() {
  form.value.cardYear.value
    ? form.value.cardYear.value.length < 2
      ? (form.value.cardYear.error = true)
      : (form.value.cardYear.error = false)
    : (form.value.cardYear.error = true);
}

function validateMonth() {
  if (+form.value.cardMonth.value === 0) form.value.cardMonth.value = "";

  if (
    form.value.cardMonth.value?.length === 1 &&
    +form.value.cardMonth.value <= 9
  ) {
    form.value.cardMonth.value = "0" + form.value.cardMonth.value;
  }

  if (+form.value.cardMonth.value > 0 && +form.value.cardMonth.value <= 12) {
    form.value.cardMonth.error = false;
  } else {
    form.value.cardMonth.error = true;
  }
}

function validateCvc() {
  if (form.value.cardCvc.value)
    form.value.cardCvc.value.length < 3
      ? (form.value.cardCvc.error = true)
      : (form.value.cardCvc.error = false);
  else form.value.cardCvc.error = true;
}

function validateCardNumber() {
  if (form.value.cardNumber.value) {
    form.value.cardNumber.value.replace(/\s/g, "").length === 16
      ? (form.value.cardNumber.error = false)
      : (form.value.cardNumber.error = true);
  } else {
    form.value.cardNumber.error = true;
  }
}

function validateCardholderName() {
  form.value.cardholderName.value = form.value.cardholderName.value?.trim();

  form.value.cardholderName.value
    ? (form.value.cardholderName.error = false)
    : (form.value.cardholderName.error = true);
}
</script>

<style lang="scss" scoped>
.card-form-active {
  display: flex;
  align-items: center;
  width: 100%;
  justify-content: center;
  height: calc(100vh - 240px);
  padding: 0 20px;
}

.card-form {
  width: 400px;
  &__error {
    position: absolute;
    top: 55px;
    left: 20px;
    padding: 0 10px;
    font-size: 12px;
    color: red;
    background-color: #fff;
    pointer-events: none;
  }

  &__inputBox {
    position: relative;
    width: 100%;
    margin: 0 0 20px 0;

    &:last-child {
      .card-form__error {
        left: 10px;
      }
    }
  }

  &__title {
    margin-bottom: 5px;
  }

  &__input {
    width: 100%;
    border: 1px solid rgba(0, 0, 0, 0.3);
    outline: none;
    border-radius: 5px;

    &:focus {
      border: 1px solid rgb(15, 0, 127);
    }
  }

  &__input_month-year {
    + :last-child {
      margin-left: 20px;
      margin-right: 20px;
    }
  }

  &__flexbox {
    display: flex;
    flex-direction: row;
    justify-content: space-between;

    .card-form__inputBox:first-child {
      .card-form__error {
        padding: 0;
        margin: 0;
        top: 65px;
        left: 0px;
      }
    }
  }

  .wrong {
    border: 1px solid red;
  }
}

@media (min-width: 900px) {
  .card-form {
    margin-top: 50px;
  }
}

@media (min-width: 1300px) {
  .card-form-active {
    height: 100vh;
    margin: 0;
    align-items: center;
  }
}
</style>
