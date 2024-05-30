<script setup>
import { ref } from 'vue';

const item1 = {
  name: '1',
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
const z = [0.7, 0.9, 0.6, 0.8, 0.9];
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
</script>

<template>
  <div class="drag-drop-container">
    <div class="container-one">
      <div
        class="item"
        v-for="(item, index) in containerOne"
        :style="{
          left: x[index] + 'px',
          top: y[index] + 'px',
          scale: z[index],
        }"
        :key="index"
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
        v-for="(item, index) in containerTwo"
        :style="{
          scale: z[index],
        }"
        :key="index"
        draggable="true"
        v-on:dragstart="handleDragStart($event, item)"
      ></div>
    </div>
    <div class="basket"></div>
  </div>
</template>

<style scoped>
.drag-drop-container {
  position: relative;
  width: 1100px;
}
.container-one {
  position: relative;
  background-image: url(/public/img/forest.png);
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
  width: 1100px;
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
/* .item1 {
  display: inline-block;
  background-image: url(/public/img/mushroom.png);
  background-repeat: no-repeat;
  background-size: cover;
  width: 63px;
  height: 73px;
} */
.position1 {
  position: absolute;
  top: 300px;
  right: 800px;
}
</style>
