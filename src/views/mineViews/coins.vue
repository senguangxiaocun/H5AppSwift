<template>
  <div class="page">
    <div class="top-header">
      <BackButton />
      <span class="edit-title">My diamonds</span>
    </div>

    <!-- 金币盒子UI -->
    <div class="coin-box">
      <img src="@/assets/coin.png" alt="coin" class="coin-icon" />
      <div class="coin-box-content">
        <div class="coin-box-header">My diamonds</div>
        <span class="coin-number">{{ currentUserStore.currentUser.coins }}</span>
      </div>
    </div>
    <!-- <div class="coinbgc"></div> -->
    <!-- 金币列表 -->
    <div class="coins">
      <div class="coins-grid">
        <div
          v-for="(item, index) in otherStore.other.coinsSetting"
          :key="index"
          class="coin-item"
          :class="{ 'coin-item-selected': selectedIndex === index }"
          @click="
            () => {
              selectedIndex = index;
              showGoToLoginPay(item);
            }
          "
        >
          <div class="coin-left">
            <img src="@/assets/coin.png" class="coin-item-icon" />
            <span
              class="coin-count"
              :class="{ 'coin-count-selected': selectedIndex === index }"
              >{{ item.cions }}</span
            >
          </div>

          <div class="coin-right">
            <span
              class="coin-price"
              :class="{ 'coin-price-selected': selectedIndex === index }"
              >{{ item.money }} $</span
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import BackButton from "@/components/back.vue";
import { useOtherStore } from "@/stores/other";
import { useCurrentUserStore } from "@/stores/currentUser";
// import { useUserStore } from "@/stores/user";
import { sendPaymentToIOS, sendShowToastToIOS } from "@/utils/iosBridge";

// 游客购买限制
function showGoToLoginPay(item) {
  if (currentUserStore.currentUser.isguest == 1) {
    sendShowToastToIOS("guestLogin");
    return;
  }
  handleCoinClick(item);
}

const otherStore = useOtherStore();
const currentUserStore = useCurrentUserStore();
// const userStore = useUserStore();

const selectedIndex = ref(-1);

function handleCoinClick(item) {
  // item.key 或 item.id 作为支付标识
  const payKey = item.key;

  // 调用 iOS 支付
  sendPaymentToIOS(payKey);
}
</script>

<style scoped>
.page {
  position: relative;
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 1);
  background-image: url("@/assets/pagebgc.png");
  background-size: cover; /* 等比缩放覆盖 */
  background-position: center; /* 居中显示 */
  background-repeat: no-repeat;
  overflow: hidden;
  /* display: flex;
  flex-direction: column; */
  box-sizing: border-box;
}

.top-header {
  display: flex;
  align-items: center;
  gap: calc(100vw * 16 / 375);
  padding: calc(100vh * 58 / 812) calc(100vw * 20 / 375) 0;
}

.edit-title {
  font-family: "Poppins", sans-serif;
  font-size: calc(100vw * 20 / 375);
  font-weight: 700;
  background: rgba(31, 40, 0, 1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* 金币盒子外层 */
.coin-box {
  width: auto;
  /* height: calc(100vh * 110 / 812); */
  border-radius: calc(100vw * 20 / 375);
  background: rgba(201, 238, 64, 1);
  overflow: hidden;
  margin: calc(100vh * 20 / 812) calc(100vw * 20 / 375) 0;
  display: flex;
  flex-direction: row;
  /* justify-content: center; */
  gap: calc(100vh * 32 / 812);
  padding: calc(100vw * 13 / 375) calc(100vw * 20 / 375);
}

/* 上部分标题 */
.coin-box-header {
  font-family: "Poppins", sans-serif;
  font-size: calc(100vw * 18 / 375);
  font-weight: 400;
  letter-spacing: 0px;
  line-height: calc(100vw * 20.79 / 375);
  color: rgb(0, 0, 0);
  padding-top: calc(100vw * 12 / 375);
  padding-bottom: calc(100vw * 15 / 375);
}

/* 下部分盒子内容 */
.coin-box-content {
  height: 100%; /* 确保有足够高度让居中生效 */
  align-items: center;
  justify-content: center;

  width: fit-content; /* 核心：宽度自适应内容 */
  max-width: 100%; /* 避免内容超出父容器 */
}

/* coin 图标 */
.coin-icon {
  width: calc(100vw * 78 / 375);
  height: calc(100vh * 78 / 812);
}

/* 金币数量文字 */
.coin-number {
  font-family: "Poppins", sans-serif;
  font-size: calc(100vw * 26 / 375);
  font-weight: 700;
  letter-spacing: 0px;
  line-height: calc(100vw * 17.41 / 375);
  color: rgb(0, 0, 0);
}

.coinbgc {
  position: absolute;
  top: calc(100vh * 16 / 812);
  right: calc(100vh * 27 / 812);
  width: calc(100vw * 167 / 375);
  height: calc(100vh * 297 / 812);
  background-image: url("@/assets/coinsbgc.png");
  background-size: cover; /* 等比缩放覆盖 */
  background-position: center; /* 居中显示 */
  background-repeat: no-repeat;
  overflow: hidden;
  pointer-events: none; /* 点击穿透，不拦截点击事件 */
}

.coins {
  height: calc(100% - calc(100vh * 285 / 812));
  margin-top: calc(100vh * 30 / 812);
  padding: 0 calc(100vw * 20 / 375) calc(100vh * 34 / 812);
  overflow-y: auto;
}

.coin-list {
  display: flex;
  flex-direction: column;
  gap: calc(100vh * 12 / 812);
}

.coins-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3列，等宽 */
  gap: calc(100vw * 12 / 375); /* 网格间距 */
}

.coin-item {
  height: calc(100vh * 100 / 812);
  border-radius: calc(100vw * 20 / 375);
  background: rgba(201, 238, 64, 0.2);
  box-shadow: 0px calc(100vw * 2 / 375) calc(100vw * 4 / 375) rgba(0, 0, 0, 0.06);
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  gap: calc(100vw * 10 / 375);
  padding: 0 calc(100vw * 10 / 375);
  box-sizing: border-box;
}

.coin-item-selected {
  border-radius: calc(100vw * 20 / 375);
  background: rgba(201, 238, 64, 1);
}

.coin-left {
  display: flex;
  align-items: center;
  gap: calc(100vw * 8 / 375);
}

.coin-item-icon {
  width: calc(100vw * 24 / 375);
  height: calc(100vh * 24 / 812);
}

.coin-count {
  font-family: "Poppins", sans-serif;
  font-size: calc(100vw * 16 / 375);
  font-weight: 700;
  line-height: calc(100vw * 18.48 / 375);
  color: rgb(0, 0, 0);
}

.coin-count-selected {
  color: #000000;
}

.coin-right {
  display: flex;
  align-items: center;
  gap: calc(100vw * 10 / 375);
}

.coin-price {
  font-family: "Poppins", sans-serif;
  font-size: calc(100vw * 14 / 375);
  font-weight: 400;
  line-height: calc(100vw * 15.23 / 375);
  color: rgb(0, 0, 0);
}

.coin-price-selected {
  color: #000000;
}
</style>
