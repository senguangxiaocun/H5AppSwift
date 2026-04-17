<template>
  <div class="coinnot-box">
    <div class="recharge-button" @click="handleRecharge">Recharge</div>
  </div>
</template>

<script setup>
import { defineEmits } from "vue";

const emit = defineEmits(["recharge"]);

function handleRecharge() {
  emit("recharge", true);
}
</script>

<style scoped>
.coinnot-box {
  width: calc(100vw * 280 / 375);
  height: calc(100vh * 293 / 812);
  background-image: url("@/assets/coinnot.png");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  position: relative;
}

.recharge-button {
  position: absolute;
  bottom: calc(100vh * 34 / 812);
  left: 50%;
  transform: translateX(-50%);
  width: calc(100vw * 190 / 375);
  height: calc(100vh * 54 / 812);
  border-radius: calc(100vw * 40 / 375);
  background: rgba(201, 238, 64, 1);
  backdrop-filter: blur(10px);

  display: flex;
  justify-content: center;
  align-items: center;
  font-family: "Poppins", sans-serif;
  font-size: calc(100vw * 16 / 375);
  font-weight: 600;
  line-height: calc(100vw * 23.1 / 375);
  letter-spacing: 0;
  color: rgb(0, 0, 0);
}
</style>
