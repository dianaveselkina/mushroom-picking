<script setup>
import { ref } from 'vue';

const item1 = {
  id: 1,
  x: 100,
  y: 600,
  z: 0.7,
  r: 3,
  d: 50,
  f: 358,
  g: 17,
};
const item2 = {
  id: 2,
  x: 300,
  y: 800,
  z: 0.9,
  r: 4,
  d: 1,
  f: 13,
  g: 15,
};
const item3 = {
  id: 3,
  x: 860,
  y: 580,
  z: 0.7,
  r: 5,
  d: 30,
  f: 42,
  g: 30,
};
const item4 = {
  id: 4,
  x: 128,
  y: 1023,
  z: 0.8,
  r: 6,
  d: 75,
  f: 11,
  g: 34,
};
const item5 = {
  id: 5,
  x: 500,
  y: 1100,
  z: 0.9,
  r: 7,
  d: 123,
  f: 49,
  g: 35,
};

let containerOne = ref([item1, item2, item3, item4, item5]);
let containerTwo = ref([]);

const handleDragStart = (event, itemData) => {
  event.dataTransfer.setData('application/json', JSON.stringify(itemData));
};

const handleDrop = (event, targetContainer) => {
  const itemData = JSON.parse(event.dataTransfer.getData('application/json'));
  console.log(targetContainer === containerTwo.value);
  if (targetContainer === containerTwo.value) {
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
        v-for="item in containerOne"
        :style="{
          left: item.x + 'px',
          top: item.y + 'px',
          scale: item.z,
        }"
        :key="item.id"
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
        v-for="item in containerTwo"
        :style="{
          scale: item.z,
          zIndex: item.r,
          left: item.d + 'px',
          rotate: item.f + 'deg',
          top: item.g + 'px',
        }"
        :key="item.id"
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
  height: 1100px;
}
.item {
  position: absolute;
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
  cursor: pointer;
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
  cursor: pointer;
  padding: 20px;
  font-size: 24px;
  border-radius: 16px;
}
</style>
