
<script setup>
import { defineProps } from 'vue'
defineProps({
  title: {
    type: String,
    default: 'Empty component',
    required: false
  }

})
</script>

<template>
  <div class="card">
    <div class="card-header">
      <h2>{{ title }}</h2>
    </div>
  </div>
</template>

