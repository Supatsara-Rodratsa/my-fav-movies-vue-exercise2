<script setup>
const props = defineProps({ titles: Array, placeholder: String, currentSelectedItem: String });
const emit = defineEmits(['updateSelectedItem']);

function onSelectedItem(item) {
  emit('updateSelectedItem', item);
}

</script>

<template>
  <select v-model="currentSelectedItem" @change="onSelectedItem(currentSelectedItem)">
    <option disabled value="">{{ placeholder || 'Please select a favorite!' }}</option>
    <template v-for="item in titles" v-bind:key="item">
      <option>{{ item }}</option>
    </template>
  </select>
</template>

<style scoped>

select {
  height: 40px;
  border-radius: 30px;
  padding: 0 20px;
  appearance: none;
  background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
  background-repeat: no-repeat;
  background-position: right 1rem center;
  background-size: 1em;
  width: 300px;
}

select:focus {
  outline: none;
}
</style>
