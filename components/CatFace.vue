<template>
  <div class="cat-face">
    <img v-if="!isActive" src="~/assets/zzz.png" alt="Zzz. The cat is asleep." class="zzz">
    <div class="ears">
      <div class="ear-left">
        <div class="ear ear-left-stick"></div>
        <div class="ear ear-right-stick"></div>
      </div>
      <div class="ear-right">
        <div class="ear ear-left-stick"></div>
        <div class="ear ear-right-stick"></div>
      </div>
    </div>
    <!-- <div v-if="isActive" class="eyes">
      <div class="eye eye-left"></div>
      <div class="eye eye-right"></div>
    </div>-->
    <div class="nose"></div>
    <div class="whiskers">
      <div class="whisker whisker-left whisker-left-top"></div>
      <div class="whisker whisker-left whisker-left-mid"></div>
      <div class="whisker whisker-left whisker-left-bot"></div>
      <div class="whisker whisker-right whisker-right-top"></div>
      <div class="whisker whisker-right whisker-right-mid"></div>
      <div class="whisker whisker-right whisker-right-bot"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isActive: Boolean
  }
};
</script>

<style>
:root {
  --cat-color: #333;
  --eye-size: 8px;
  --eye-color: #ffbbca;
  --whisker-width: calc(var(--square-size) / 3);
  --whisker-height: 2px;
  --whisker-color: var(--cat-color);
  --whisker-top-pos: 110px;
  --nose-size: 10px;
  --nose-color: var(--cat-color);
  --ear-width: 30px;
  --ear-height: var(--whisker-height);
  --ear-color: var(--cat-color);
}

.cat-face {
  position: relative;
  width: 100%;
  height: 100%;
}
.zzz {
  position: absolute;
  right: 5px;
  top: 10px;
  width: auto;
  height: 30px;
}
.eyes {
  width: 100%;
  top: 85px;
  position: absolute;
}
.eye {
  position: absolute;
  background-color: var(--eye-color);
  height: var(--eye-size);
  width: var(--eye-size);
  border-radius: 100%;
}
.eye-left {
  left: 70px;
}
.eye-right {
  left: 120px;
}
.whiskers {
  position: absolute;
  top: var(--whisker-top-pos);
  width: 100%;
}
.whisker {
  position: absolute;
  background-color: var(--whisker-color);
  height: var(--whisker-height);
  width: var(--whisker-width);
}
.whisker-left {
  left: 15px;
}

.whisker-right {
  right: 15px;
}

.whisker-left-top {
  top: 0px;
  transform: rotate(15deg);
}

.whisker-left-mid {
  top: 20px;
}

.whisker-left-bot {
  top: 40px;
  transform: rotate(-15deg);
}

.whisker-right-top {
  top: 0px;
  transform: rotate(165deg);
}

.whisker-right-mid {
  top: 20px;
}

.whisker-right-bot {
  top: 40px;
  transform: rotate(-165deg);
}
.nose {
  position: absolute;
  left: calc(50% - var(--nose-size) / 2);
  top: calc(var(--whisker-top-pos) + 15px);
  width: var(--nose-size);
  height: var(--nose-size);
  border-radius: 100%;
  background-color: var(--nose-color);
}
.ears {
  position: absolute;
  width: 100%;
  top: 50px;
}
.ear-left {
  position: absolute;
  left: 40px;
}
.ear-right {
  position: absolute;
  left: 120px;
}
.ear {
  position: absolute;
  background-color: var(--ear-color);
  width: var(--ear-width);
  height: var(--ear-height);
}
.ear-left-stick {
  transform: rotate(-65deg);
}
.ear-right-stick {
  left: 12px;
  transform: rotate(65deg);
}
</style>
