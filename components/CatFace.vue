<template>
  <div class="cat-face">
    <div class="eyes">
      <div class="eye eye-left"></div>
      <div class="eye eye-right"></div>
    </div>
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
  --whisker-width: calc(var(--square-size) / 3);
  --whisker-height: 2px;
  --whisker-color: black;
  --eye-width: 20px;
  --eye-height: var(--whisker-height);
  --eye-color: var(--whisker-color);
}

.cat-face {
  position: relative;
}
.whiskers {
  position: absolute;
  top: 110px;
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
.eyes {
  position: absolute;
  width: 100%;
  top: 60px;
}
.eye {
  position: absolute;
  left: 60px;
  background-color: var(--eye-color);
  width: var(--eye-width);
  height: var(--eye-height);
  transform: rotate(-65deg);
}
.eye-left:after {
  position: absolute;
  left: 10px;
  background-color: var(--eye-color);
  width: var(--eye-width);
  height: var(--eye-height);
  transform: rotate(-65deg);
}
</style>
