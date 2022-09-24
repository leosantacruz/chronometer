<script setup>
import { ref } from "vue";

const logs = ref([]);
const timer = ref(0);
let interval = null;
const pauseStatus = ref(true);
const start = () => {
  if (logs.value.length < 21) {
    interval = setInterval((r) => {
      timer.value++;
    }, 100);
  }
};
const add = () => {
  clearInterval(interval);

  if (logs.value.length <= 20) {
    logs.value.push(timer.value / 10);
    start();
  }
  if (logs.value.length == 21) {
    playSound();
  }
  timer.value = 0;
};

const reset = () => {
  logs.value = [];
  timer.value = 0;
  clearInterval(interval);
};
const playSound = () => {
  const sound = new URL("/sound.mp3", import.meta.url);
  new Audio(sound.href).play();
};
const pause = () => {
  if (pauseStatus.value) {
    clearInterval(interval);
  } else {
    start();
  }
  pauseStatus.value = !pauseStatus.value;
};
</script>

<template>
  <div id="container-grid">
    <div id="header">
      <div class="dynamic-island">
        <div>{{ timer }}</div>
        <div class="actions">
          <div @click="pause"><img src="/pause.svg" alt="" /></div>
          <div @click="reset"><img src="/refresh-ccw.svg" alt="" /></div>
        </div>
        <div>46</div>
      </div>
    </div>
    <div id="logs">
      <div v-for="index in 20" :key="index">{{ logs[index] }}</div>
    </div>
    <div id="action" @click="add()" :class="{ forbidden: logs.length >= 21 }">
      <div class="button"><img src="/plus-square.svg" alt="" /></div>
    </div>
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");
#container-grid {
  font-family: "Roboto", sans-serif;
  height: 100vh;
  display: grid;
  grid-template-rows: 70px 250px 1fr;
}
#header {
  background-color: rgb(0, 225, 255);
}
#logs {
  background-color: rgb(0, 225, 255);
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(4, 1fr);
  column-gap: 2px;
  row-gap: 2px;
}
#logs div {
  background-color: rgb(18, 18, 18);
  display: flex;
  justify-content: center;
  align-items: center;
  color: #10d4ff;
  font-size: 18px;
}

.dynamic-island {
  background-color: black;
  color: #fff;
  width: 250px;
  margin: 10px auto;
  border-radius: 20px;
  display: flex;
  flex-direction: row;
  -webkit-justify-content: space-around;
  justify-content: space-around;
  align-items: center;
  padding-top: 10px;
  padding-bottom: 10px;
}
.dynamic-island .actions {
  display: flex;
  flex-direction: row;
}
.dynamic-island .actions div {
  margin-right: 15px;
  margin-left: 15px;
}
#action {
  background-color: black;
  width: 100%;
  display: flex;
  margin: 0 auto;
  align-items: center;
  justify-content: center;
  background-color: black;
  transition: all 0.3s linear;
}
#action .button {
  transition: all 0.1s linear;
}
#action .button:active {
  transform: scale(0.9);
}
.forbidden {
  opacity: 0.5;
}
</style>
