<template>
  <div class="cat-face">
    <div class="eyes">
      <div class="eye-left">
        <div class="eye eye-left-stick"></div>
        <div class="eye eye-right-stick"></div>
      </div>
      <div class="eye-right">
        <div class="eye eye-left-stick"></div>
        <div class="eye eye-right-stick"></div>
      </div>
    </div>
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
export default {};
</script>

<style>
:root {
  --cat-color: #333;
  --whisker-width: calc(var(--square-size) / 3);
  --whisker-height: 2px;
  --whisker-color: var(--cat-color);
  --whisker-top-pos: 110px;
  --nose-size: 10px;
  --nose-color: var(--cat-color);
  --eye-width: 30px;
  --eye-height: var(--whisker-height);
  --eye-color: var(--cat-color);
}

.cat-face {
  position: relative;
  width: 100%;
  height: 100%;
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
.eyes {
  position: absolute;
  width: 100%;
  top: 50px;
}
.eye-left {
  position: absolute;
  left: 40px;
}
.eye-right {
  position: absolute;
  left: 120px;
}
.eye {
  position: absolute;
  background-color: var(--eye-color);
  width: var(--eye-width);
  height: var(--eye-height);
}
.eye-left-stick {
  transform: rotate(-65deg);
}
.eye-right-stick {
  left: 12px;
  transform: rotate(65deg);
}
</style>
