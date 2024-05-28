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
        v-for="item in containerOne"
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
        class="item1"
        v-for="item in containerTwo"
        :key="item.id"
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
  bottom: 545px;
  left: 443px;
  width: 230px;
  height: 130px;
}
.basket {
  background-image: url(/public/img/basket.png);
  background-repeat: no-repeat;
  background-size: cover;
  z-index: 1;
  position: absolute;
  bottom: 400px;
  left: 400px;
  width: 314px;
  height: 200px;
}
.item {
  position: absolute;
  top: 200px;
  right: 200px;
  display: inline-block;
  background-image: url(/public/img/mushroom.png);
  background-repeat: no-repeat;
  background-size: cover;
  width: 63px;
  height: 73px;

  cursor: pointer;
}
.item1 {
  display: inline-block;
  background-image: url(/public/img/mushroom.png);
  background-repeat: no-repeat;
  background-size: cover;
  width: 63px;
  height: 73px;
}
</style>
