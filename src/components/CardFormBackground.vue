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
        <div v-if="cardNumber" class="card-form-background__card-number">
          {{ cardNumber }}
        </div>
        <div v-if="!cardNumber" class="card-form-background__card-number">
          0000 0000 0000 0000
        </div>

        <div class="flexbox">
          <div
            v-if="cardholderName"
            class="card-form-background__cardholder-name"
          >
            {{ cardholderName?.toUpperCase() }}
          </div>

          <div
            v-if="!cardholderName"
            class="card-form-background__cardholder-name"
          >
            JANE APPLESEED
          </div>

          <div class="card-form-background__card-exp-date">
            <span v-if="!cardMonth && !cardYear">09</span>
            <span v-else>{{ cardMonth }}</span
            >/
            <span v-if="!cardYear && !cardMonth">25</span>
            <span v-else>{{ cardYear }}</span>
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
        <div v-if="!cardCvc" class="card-form-background__card-cvc">000</div>
        <div v-if="cardCvc" class="card-form-background__card-cvc">
          {{ cardCvc }}
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
  cardholderName: String,
  cardNumber: String,
  cardMonth: String,
  cardYear: String,
  cardCvc: String,
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
}

.scaledDown {
  scale: 0.9;
  filter: blur(2px);
}

.card-form-background {
  height: 100vh;
  width: 30%; //default width = 483px
  background-image: url("../assets/images/bg-main-desktop.png");
  background-repeat: no-repeat;
  background-size: cover;
  display: flex;
  align-items: center;

  .card-form-cards-wrapper {
    margin-left: 15%;
  }

  &__card-front {
    color: #fff;
    margin-bottom: 30px;
    background-image: url("../assets/images/bg-card-front.png");
    width: 447px;
    height: 245px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: all 0.5s ease-in-out;
  }

  &__card-back {
    color: #fff;
    margin-left: 100px;
    background-image: url("../assets/images/bg-card-back.png");
    width: 447px;
    height: 245px;
    padding-right: 60px;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    transition: all 0.5s ease-in-out;
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

  &__circles {
    display: flex;
    align-items: center;
    margin-bottom: 70px;
  }

  .circle-full {
    width: 50px;
    height: 50px;
    background-color: #fff;
    border-radius: 50%;
  }

  .circle-border {
    margin-left: 20px;
    width: 20px;
    height: 20px;
    border: 1px solid #fff;
    border-radius: 50%;
  }
}
</style>
