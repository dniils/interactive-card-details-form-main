<template>
  <div class="card-form-active">
    <form @submit.prevent="submitForm" class="card-form">
      <div class="card-form__inputBox">
        <div class="card-form__title">Cardholder Name</div>
        <input
          @blur="validateCardholderName"
          @input="nameInputHandler"
          v-model="cardholderName"
          class="card-form__input"
          :class="{ wrong: !correctName }"
          placeholder="Jane Appleseed"
          type="text"
          required
        />

        <div v-if="!correctName" class="card-form__error">
          Please, enter your Name and Surname
        </div>
      </div>

      <div class="card-form__inputBox">
        <div class="card-form__title">Card Number</div>
        <input
          @blur="validateCardNumber"
          @input="cardNumberInputHandler"
          v-model="cardNumber"
          class="card-form__input"
          :class="{ wrong: !correctCardNumber }"
          type="text"
          placeholder="0000 0000 0000 0000"
          required
        />
        <div v-if="!correctCardNumber" class="card-form__error">
          Only 16 digits cards allowed
        </div>
      </div>

      <div class="card-form__flexbox">
        <div class="card-form__inputBox">
          <div class="card-form__title">Exp. Date (MM/YY)</div>
          <div class="card-form__flexbox">
            <input
              @blur="validateMonth"
              @input="cardMonthInputHandler"
              v-model="cardMonth"
              class="card-form__input card-form__input_month-year"
              :class="{ wrong: !correctMonth }"
              placeholder="MM"
              required
            />

            <input
              @blur="validateYear"
              @input="cardYearInputHandler"
              v-model="cardYear"
              class="card-form__input card-form__input_month-year"
              :class="{ wrong: !correctYear }"
              placeholder="YY"
              required
            />
            <div v-if="!correctMonth || !correctYear" class="card-form__error">
              Check format
            </div>
          </div>
        </div>

        <div class="card-form__inputBox">
          <div class="card-form__title">CVC</div>
          <input
            @blur="validateCvc"
            @input="cardCvcInputHandler"
            v-model="cardCvc"
            class="card-form__input card-form__input_cvc"
            :class="{ wrong: !correctCvc }"
            placeholder="123"
            required
          />
          <div v-if="!correctCvc" class="card-form__error">
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

let cardholderName = ref();
let cardNumber = ref();
let cardCvc = ref();
let cardMonth = ref();
let cardYear = ref();

let correctName = ref(true);
let correctCardNumber = ref(true);
let correctCvc = ref(true);
let correctMonth = ref(true);
let correctYear = ref(true);

const emit = defineEmits("inputEvent");

function emitVariables() {
  emit("inputEvent", cardholderName, cardNumber, cardMonth, cardYear, cardCvc);
}

function submitForm() {
  emit(
    "submitEvent",
    correctName,
    correctCardNumber,
    correctCvc,
    correctMonth,
    correctYear
  );
}

// Allow names like "Eugene Joseph-le-brun"
function nameInputHandler() {
  correctName.value = true;
  cardholderName.value = cardholderName.value
    .replace(/([^a-z ]+)/gi, "")
    .replace(/\s+/g, " ")
    .split(" ")
    .map((word) => {
      if (word) return word[0].toUpperCase() + word.substring(1).toLowerCase();
    })
    .join(" ");

  emitVariables();
}

function cardNumberInputHandler() {
  correctCardNumber.value = true;

  cardNumber.value = cardNumber.value
    .replace(/[^0-9]*/g, "")
    .slice(0, 16)
    .split("")
    .map((digit, i) => ((i + 1) % 4 === 1 ? " " + digit : digit))
    .join("")
    .slice(1);

  emitVariables();
}

function cardCvcInputHandler() {
  cardCvc.value = cardCvc.value.replace(/[^0-9]*/g, "").slice(0, 3);
  emitVariables();
}

function cardMonthInputHandler() {
  cardMonth.value = cardMonth.value.replace(/[^0-9]*/g, "").slice(0, 2);
  emitVariables();
}

function cardYearInputHandler() {
  cardYear.value = cardYear.value.replace(/[^0-9]/g, "").slice(0, 2);
  emitVariables();
}

function validateYear() {
  cardYear.value
    ? cardYear.value.length < 2
      ? (correctYear.value = false)
      : (correctYear.value = true)
    : (correctYear.value = false);
}

function validateMonth() {
  if (cardMonth.value?.length === 1 && +cardMonth.value !== 0) {
    cardMonth.value = "0" + cardMonth.value;
  }

  if (+cardMonth.value > 0 && +cardMonth.value <= 12) {
    correctMonth.value = true;
  } else {
    correctMonth.value = false;
  }
}

function validateCvc() {
  if (cardCvc.value)
    cardCvc.value.length < 3
      ? (correctCvc.value = false)
      : (correctCvc.value = true);
  else correctCvc.value = false;
}

function validateCardNumber() {
  if (cardNumber.value) {
    cardNumber.value.replace(/\s/g, "").length === 16
      ? (correctCardNumber.value = true)
      : (correctCardNumber.value = false);
  } else {
    correctCardNumber.value = false;
  }
}

function validateCardholderName() {
  cardholderName.value = cardholderName.value?.trim();

  cardholderName.value
    ? (correctName.value = true)
    : (correctName.value = false);
}
</script>

<style lang="scss" scoped>
.card-form-active {
  display: flex;
  align-items: center;
  width: calc(100vw - 30%);
  justify-content: center;
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
</style>
