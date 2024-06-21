<script setup>
import { ref } from 'vue';

const item1 = {
  id: 1,
};
const item2 = {
  id: 2,
};
const item3 = {
  id: 3,
};
const item4 = {
  id: 4,
};
const item5 = {
  id: 5,
};
const x = [100, 300, 810, -128, 500];
const y = [600, 800, 530, 1023, 1100];
const z = [0.7, 0.9, 0.7, 0.8, 0.9];
const r = [3, 4, 5, 6, 7];
const d = [34, 1, -41, 46, 26];
const f = [-37, 13, 42, -46, 49];
const g = [1, 1, 1, -44, -42];
let containerOne = ref([item1, item2, item3, item4, item5]);
let containerTwo = ref([]);

const handleDragStart = (event, itemData) => {
  event.dataTransfer.setData('application/json', JSON.stringify(itemData));
};

const handleDrop = (event, targetContainer) => {
  const itemData = JSON.parse(event.dataTransfer.getData('application/json'));

  if (targetContainer === containerOne.value) {
    containerTwo.value = containerTwo.value.filter((i) => i.id !== itemData.id);
  } else if (targetContainer === containerTwo.value) {
    containerOne.value = containerOne.value.filter((i) => i.id !== itemData.id);
  }
  targetContainer.push(itemData);
};
const animated = ref(false);
const add = () => {
  animated.value = !animated.value;
};
const refreshPage = () => {
  location.reload();
};
const modalOpen = ref(false);

const toggleVisibility = () => {
  setTimeout(() => {
    modalOpen.value = true;
  }, '1500');
};
</script>

<template>
  `
  <teleport to="body">
    <div v-if="modalOpen" class="modal">
      <img class="game-over" src="/img/game-over.png" />
      <button
        class="btn"
        type="button"
        @click="(modalOpen = false), refreshPage()"
      >
        Попробовать еще раз
      </button>
    </div>
  </teleport>
  `
  <h2>собери грибы в корзину</h2>
  <div class="drag-drop-container">
    <div :class="animated ? '' : 'active'" class="ambulance"></div>
    <div
      @mousedown="add(), toggleVisibility()"
      class="mushroom mushroom1"
    ></div>
    <div
      @mousedown="add(), toggleVisibility()"
      class="mushroom mushroom2"
    ></div>
    <div
      @mousedown="add(), toggleVisibility()"
      class="mushroom mushroom3"
    ></div>
    <div
      @mousedown="add(), toggleVisibility()"
      class="mushroom mushroom4"
    ></div>
    <div class="container-one">
      <div
        class="item"
        v-for="(item, id) in containerOne"
        :style="{
          left: x[id] + 'px',
          top: y[id] + 'px',
          scale: z[id],
        }"
        :key="id"
        draggable="true"
        v-on:dragstart="handleDragStart($event, item)"
      ></div>
    </div>

    <div
      class="container-two"
      v-on:dragover.prevent
      v-on:drop="handleDrop($event, containerTwo)"
    >
      <div
        class="item"
        v-for="(item, id) in containerTwo"
        :style="{
          scale: z[id],
          zIndex: r[id],
          left: d[id] + 'px',
          rotate: f[id] + 'deg',
          top: g[id] + 'px',
        }"
        :key="id"
        draggable="true"
        v-on:dragstart="handleDragStart($event, item)"
      ></div>
    </div>
    <div class="basket"></div>
  </div>
</template>

<style scoped>
h2 {
  text-transform: uppercase;
  text-align: center;
  font-size: 24px;
  margin-top: 18px;
}
.drag-drop-container {
  background-image: url(/public/img/forest.png);
  background-repeat: no-repeat;
  background-size: cover;
  border-radius: 24px;
  overflow: hidden;
  position: relative;
  width: 1200px;
}
.container-one {
  position: relative;
  width: 1200px;
  height: 1200px;
  padding: 10px;
}
.container-two {
  z-index: 2;
  position: absolute;
  bottom: 379px;
  left: 862px;
  width: 189px;
  height: 95px;
}
.basket {
  background-image: url(/public/img/basket.png);
  background-repeat: no-repeat;
  background-size: cover;
  z-index: 1;
  position: absolute;
  bottom: 295px;
  left: 850px;
  width: 209px;
  height: 134px;
}
.game-over {
  background-repeat: no-repeat;
  background-size: cover;
  width: 700px;
  height: 1200px;
}
.monster-enter-active {
  transition: all 1s ease-out;
}

.monster-enter-to {
  transform: scale(8);
  opacity: 1;
}
.item {
  position: relative;
  display: inline-block;
  background-image: url(/public/img/mushroom.png);
  background-repeat: no-repeat;
  background-size: cover;
  width: 63px;
  height: 73px;
  cursor: pointer;
}
.mushroom {
  position: absolute;
  z-index: 5;
  background-repeat: no-repeat;
  background-size: cover;
  width: 52px;
  height: 65px;
}
.mushroom1 {
  background-image: url(/public/img/toadstool.png);
  bottom: 65px;
  left: 300px;
}
.mushroom2 {
  background-image: url(/public/img/toadstool1.png);
  bottom: 65px;
  bottom: 200px;
  left: 984px;
}
.mushroom3 {
  background-image: url(/public/img/fly-agaric.png);
  top: 576px;
  left: 1020px;
  width: 37px;
  height: 45px;
}
.mushroom4 {
  background-image: url(/public/img/fly-agaric.png);
  top: 506px;
  left: 286px;
  width: 37px;
  height: 45px;
}
.ambulance {
  position: absolute;
  background-image: url(/public/img/ambulance.png);
  z-index: 5;
  background-repeat: no-repeat;
  background-size: cover;
  width: 173px;
  height: 120px;
  bottom: 490px;
  right: -680px;
  transform: translateX(-1200px);
  transition: all 1s;
}
.ambulance.active {
  transform: translateX(0px);
}
.modal {
  z-index: 10;
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  align-items: center;
}
.btn {
  padding: 20px;
  font-size: 24px;
  border-radius: 16px;
}
</style>
