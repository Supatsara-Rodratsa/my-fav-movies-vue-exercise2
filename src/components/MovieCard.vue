<script setup>
import Toggle from './Toggle.vue';
import StarRating from './StarRating.vue';

const props = defineProps({ movie: Object, favoriteMovie: String, watchedList: Array});
const emit = defineEmits(['updatedWatchedList']);

function getToggleUpdateValue(value) {
  emit('updatedWatchedList', value);
}

</script>

<template>
  <div class="card flex-col space-between" :style="`background-image: url(${movie.picture})`">
    <div class="toggle-container">
        <Toggle :id="movie.id" :title="movie.title" :watchedList="watchedList" @updateToggleValue="getToggleUpdateValue($event)"></Toggle>
    </div>
    <div class="content-bg flex-col-end">
      <div class="content-details flex-col">
        <div class="flex-row center space-between">
          <p class="title">{{movie.title}}</p>
          <p class="title" v-show="favoriteMovie && favoriteMovie == movie.title">😍</p>
        </div>
        <div class="flex-row space-between">
          <div class="flex-row rating">
            <StarRating :score="Number(movie.score)"></StarRating>
          </div>
          <p>Score: {{movie.score}} / 100</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .toggle-container {
    display: flex;
  }
  .title {
    font-size: 24px;
    margin-bottom: 15px;
    max-width: 90%;
    height: 70px;
    display: flex;
    align-items: end;
  }
  .card {
    height: 100%;
    width: 100%;
    background-repeat: no-repeat;
    max-height: 400px;
    background-size: contain;
    min-height: 400px;
    background: round;
  }

  .content-bg {
    height: 100%;
    width: 100%;
    background-image: linear-gradient(rgba(0,0,0,0), rgba(0,0,0,0.1),rgba(0,0,0,0.5),  rgba(0,0,0,0.7), rgba(0,0,0,1));
  }

  .content-details {
    width: 100%;
    padding: 20px;
  }

</style>