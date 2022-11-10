<script setup>
const props = defineProps({ titles: Array, currentSelectedItems: Array });
const emit = defineEmits(["updateSelectedItems"]);

function onSelectedItems(item) {
  emit("updateSelectedItems", item);
}
</script>

<template>
  <select
    v-model="currentSelectedItems"
    multiple
    @change="onSelectedItems(currentSelectedItems)"
  >
    <template v-for="title in titles" v-bind:key="title">
      <option>{{ title }}</option>
    </template>
  </select>
</template>

<style scoped>
select {
  border-radius: 10px;
}

select option {
  height: 30px;
  display: flex;
  align-items: center;
  padding: 5px 20px;
}

select[multiple]:focus option:checked {
  background: lightgray linear-gradient(0deg, lightgray 0%, lightgray 100%) !important;
}
</style>
