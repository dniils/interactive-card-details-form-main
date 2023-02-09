<template>
  <div class="card-form-background">
    <div class="card-form-cards-wrapper">
      <div
        @mouseover="mouseOverFirstCardEventHandler"
        @mouseout="mouseOutFirstCardEventHandler"
        :class="{
          scaledUp: mouseOverFirstCard,
          scaledDown: mouseOverSecondCard,
          translatedX: mouseOverFirstCard,
        }"
        class="card-form-background__card-front"
      >
        <div class="card-form-background__circles">
          <div class="circle-full"></div>
          <div class="circle-border"></div>
        </div>
        <div
          v-if="form.cardNumber?.value"
          class="card-form-background__card-number"
        >
          {{ form.cardNumber?.value }}
        </div>
        <div
          v-if="!form.cardNumber?.value"
          class="card-form-background__card-number"
        >
          0000 0000 0000 0000
        </div>

        <div class="flexbox">
          <div
            v-if="form.cardholderName?.value"
            class="card-form-background__cardholder-name"
          >
            {{ form.cardholderName?.value.toUpperCase() }}
          </div>

          <div
            v-if="!form.cardholderName?.value"
            class="card-form-background__cardholder-name"
          >
            JANE APPLESEED
          </div>

          <div class="card-form-background__card-exp-date">
            <span v-if="!form.cardMonth?.value && !form.cardYear?.value"
              >09</span
            >
            <span v-else>{{ form.cardMonth?.value }}</span
            >/
            <span v-if="!form.cardYear?.value && !form.cardMonth?.value"
              >25</span
            >
            <span v-else>{{ form.cardYear?.value }}</span>
          </div>
        </div>
      </div>

      <div
        @mouseover="mouseOverSecondCardEventHandler"
        @mouseout="mouseOutSecondCardEventHandler"
        :class="{
          scaledUp: mouseOverSecondCard,
          scaledDown: mouseOverFirstCard,
        }"
        class="card-form-background__card-back"
      >
        <div v-if="!form.cardCvc?.value" class="card-form-background__card-cvc">
          000
        </div>
        <div v-if="form.cardCvc?.value" class="card-form-background__card-cvc">
          {{ form.cardCvc?.value }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, ref } from "vue";

let mouseOverFirstCard = ref();
let mouseOverSecondCard = ref();

defineProps({
  form: Object,
});

function mouseOverFirstCardEventHandler() {
  mouseOverFirstCard.value = true;
  mouseOverSecondCard.value = false;
}

function mouseOutFirstCardEventHandler() {
  mouseOverFirstCard.value = false;
  mouseOverSecondCard.value = false;
}

function mouseOverSecondCardEventHandler() {
  mouseOverFirstCard.value = false;
  mouseOverSecondCard.value = true;
}

function mouseOutSecondCardEventHandler() {
  mouseOverFirstCard.value = false;
  mouseOverSecondCard.value = false;
}
</script>

<style lang="scss" scoped>
.scaledUp {
  scale: 1.05;
  filter: drop-shadow(0 5px 10px rgba(34, 3, 68, 0.526));
  z-index: 2 !important;
}

.scaledDown {
  scale: 0.9;
  filter: blur(2px);
  z-index: 1 !important;
}
.card-form-background {
  background-image: url("../assets/images/bg-main-mobile.png");
  background-repeat: no-repeat;
  background-size: cover;
  height: 239px;
  width: 100%;
  display: flex;
  justify-content: center;
  z-index: 0;

  .card-form-cards-wrapper {
    position: relative;
  }

  .flexbox {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  &__card-front {
    color: #fff;
    background-image: url("../assets/images/bg-card-front.png");
    background-repeat: no-repeat;
    background-size: contain;
    position: absolute;
    width: 285px;
    height: 156px;
    top: 126px;
    left: -170px;
    justify-content: space-between;
    padding: 15px;
    z-index: 2;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: all 0.5s ease-in-out;
  }

  &__card-back {
    color: #fff;
    background-image: url("../assets/images/bg-card-back.png");
    background-repeat: no-repeat;
    background-size: contain;
    position: absolute;
    margin: 0;
    width: 285px;
    height: 156px;
    top: 30px;
    left: -116px;
    padding-right: 33px;
    z-index: 1;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    transition: all 0.5s ease-in-out;
  }
  &__circles {
    display: flex;
    align-items: center;
    margin-bottom: 45px;
  }

  .circle-full {
    width: 25px;
    height: 25px;
    background-color: #fff;
    border-radius: 50%;
  }

  .circle-border {
    margin-left: 10px;
    width: 10px;
    height: 10px;
    border: 1px solid #fff;
    border-radius: 50%;
  }

  &__card-number {
    font-size: 20px;
  }

  &__card-cvc,
  &__cardholder-name,
  &__card-exp-date {
    font-size: 12px;
    span {
      font-size: 12px;
    }
  }
}

@media (max-width: 340px) {
  .card-form-background {
    &__card-front {
      left: -140px;
      padding: 10px;
      width: 228px;
      height: 125px;
    }

    &__card-back {
      left: -90px;
      width: 228px;
      height: 125px;
    }

    &__circles {
      margin-bottom: 25px;
    }

    &__card-number {
      font-size: 16px;
    }

    &__cardholder-name,
    &__card-cvc,
    &__card-exp-date {
      font-size: 10px;
      span {
        font-size: 10px;
      }
    }
  }
}

@media (min-width: 900px) {
  .card-form-background {
    &__circles {
      margin-bottom: 60px;
    }

    .circle-full {
      width: 30px;
      height: 30px;
    }

    .circle-border {
      width: 12px;
      height: 12px;
    }

    &__card-front {
      width: 342px;
      height: 187px;
      left: -225px;
    }

    &__card-back {
      width: 342px;
      height: 187px;
      padding-right: 40px;
    }

    &__card-number {
      font-size: 24px;
    }

    &__card-cvc,
    &__cardholder-name,
    &__card-exp-date {
      font-size: 14px;
      span {
        font-size: 14px;
      }
    }
  }
}

@media (min-width: 1300px) {
  .card-form-background {
    height: 100vh;
    width: 30%;
    background-image: url("../assets/images/bg-main-desktop.png");
    background-repeat: no-repeat;
    background-size: cover;
    display: flex;
    align-items: center;
    justify-content: flex-start;

    .card-form-cards-wrapper {
      width: 0;
      margin-left: 10%;
    }

    &__card-front {
      color: #fff;
      margin-bottom: 30px;
      background-image: url("../assets/images/bg-card-front.png");
      background-repeat: no-repeat;
      background-size: contain;
      width: 447px;
      height: 245px;
      padding: 20px;
      display: flex;
      flex-direction: column;
      transition: all 0.5s ease-in-out;
      position: unset;
    }

    &__card-back {
      color: #fff;
      margin-left: 100px;
      background-image: url("../assets/images/bg-card-back.png");
      background-repeat: no-repeat;
      background-size: contain;
      width: 447px;
      height: 245px;
      padding-right: 60px;
      display: flex;
      align-items: center;
      transition: all 0.5s ease-in-out;
      position: unset;
    }

    .circle-full {
      width: 50px;
      height: 50px;
    }

    .circle-border {
      width: 20px;
      height: 20px;
    }

    .flexbox {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
    }

    &__card-number {
      margin-bottom: 20px;
      font-size: 32px;
    }

    &__card-cvc,
    &__cardholder-name,
    &__card-exp-date {
      font-size: 18px;
      span {
        font-size: 18px;
      }
    }
  }
}
</style>
