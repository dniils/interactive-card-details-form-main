<template>
  <button class="main-button">
    <slot />
  </button>
</template>

<script setup></script>

<style lang="scss" scoped>
.main-button {
  border: none;
  width: 100%;
  padding: 10px 0;
  background-color: #000;
  color: #fff;
  border-radius: 5px;
  transition: background-position-y 0.2s ease-out;

  &:hover {
    cursor: pointer;
    background-image: url("@/assets/images/bg-main-desktop.png");
    background-position-y: center;
  }
}
</style>
