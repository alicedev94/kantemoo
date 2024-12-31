<template>
  <div>
    <CardCo
      class="card"
      title="[Object 1]"
      :style="{ left: `${x1}px`, top: `${y1}px`, position: 'absolute' }"
      @mousedown="startDrag(1, $event)"
    />
    <CardCo
      class="card green"
      title="[Object 2]"
      :style="{ left: `${x2}px`, top: `${y2}px`, position: 'absolute' }"
      @mousedown="startDrag(2, $event)"
    />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import CardCo from './components/CardCo.vue';

const x1 = ref(100);
const y1 = ref(100);
const x2 = ref(200);
const y2 = ref(200);
const isDragging = ref(false);
const dragIndex = ref(null);
const offsetX = ref(0);
const offsetY = ref(0);

const updatePosition = (event) => {
  if (isDragging.value) {
    if (dragIndex.value === 1) {
      x1.value = event.clientX - offsetX.value;
      y1.value = event.clientY - offsetY.value;
    } else if (dragIndex.value === 2) {
      x2.value = event.clientX - offsetX.value;
      y2.value = event.clientY - offsetY.value;
    }
  }
};

const startDrag = (index, event) => {
  isDragging.value = true;
  dragIndex.value = index;
  if (index === 1) {
    offsetX.value = event.clientX - x1.value;
    offsetY.value = event.clientY - y1.value;
  } else if (index === 2) {
    offsetX.value = event.clientX - x2.value;
    offsetY.value = event.clientY - y2.value;
  }
};

const stopDrag = () => {
  isDragging.value = false;
  dragIndex.value = null;
};

onMounted(() => {
  window.addEventListener('mousemove', updatePosition);
  window.addEventListener('mouseup', stopDrag);
});

onUnmounted(() => {
  window.removeEventListener('mousemove', updatePosition);
  window.removeEventListener('mouseup', stopDrag);
});
</script>

<style scoped>
.card {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
  border: 4px solid #42b883aa;
  margin: 1rem;
  text-transform: uppercase;
}

.card:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
  cursor: pointer;
}

.card.green:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
