<script setup>
/*
I tried to create a handy webapp to keep track of all the movies I've watched this year and which was my favorite, but form bindings are confusing and it's not working properly :(

Can you help me fix it?

level 1: Update the movie list with a v-for loop so all movies in the provided file movies.json are rendered. Feel free to update/change the list to your likes, or to add more info for each movie.

level 2: Create a small "movie card" for each movie object in the list and display in it the title, a small poster picture, and the score of the movie.

level 3: Remove the duplicated & hardcoded code in the form binding for "My fav movie" selector, and use a v-for loop and attribute bindings to correctly build the options. (Note: you can change the binding variable 'favoriteMovie' below if needed)

level 4: I want it to be easy to see which one is my current favorite, so please add a "😍" emoji next to the title to my favorite movie in the list.

level 5: The score of each film is represented by a string with a value between "0" and "100". We can do better! Let's show between 1 to 5 stars in each film card, according to the score (tip: use a v-for loop here).

level 6: Fix the multiselectors of the movies I've watched this year. Remove the duplicated & hardcoded code in the form binding for both multi selectors, and use v-for loops and attribute bindings to correctly build the options. Keep in mind they should be synchronized! (keep using the same binding to the variable "watchedList" for both fields).

level 7: The multiselectors are nice, but it would be great to also have a checkbox on each film card to mark it as watched/not watched. Of course, it should be synchronized with the "watchedList" array!

bonus level: Apply your CSS magic and designer's touch to make the app look great!
*/

import { ref } from "vue";
import movies from "./assets/movies.json";
import MovieCard from "./components/MovieCard.vue";
import Toggle from "./components/Toggle.vue";
import SingleSelection from "./components/SingleSelection.vue";
import MultipleSelection from "./components/MultipleSelection.vue";

const favoriteMovie = ref("");
const watchedList = ref([]);

function getUpdatedWatchedList(value) {
  watchedList.value = value;
}

function getAllTitles() {
  return movies.map((movie) => movie.title);
}

function updateFavoriteMovie(value) {
  favoriteMovie.value = value;
}
</script>

<template>
  <link
    rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css"
  />
  <h3>Movie list</h3>
  <div class="grid">
    <div v-for="item in movies" v-bind:key="item.id">
      <MovieCard
        :movie="item"
        :favoriteMovie="favoriteMovie"
        :watchedList="watchedList"
        @updatedWatchedList="getUpdatedWatchedList($event)"
      ></MovieCard>
    </div>
  </div>
  <div class="line"></div>
  <h3>Controls</h3>
  <div class="flex-col center">
    <div class="text-header">
      My fav movie:
      <span class="normal-text"
        >&nbsp;{{ favoriteMovie || "none chosen yet :(" }}</span
      >
    </div>
    <div class="mrg-20 fav-dropdown">
      <SingleSelection
        :titles="getAllTitles()"
        :currentSelectedItem="favoriteMovie"
        @updateSelectedItem="updateFavoriteMovie($event)"
      ></SingleSelection>
    </div>
  </div>
  <br /><br />
  <div class="flex-col center">
    <div class="text-header watched-movie">
      Movies watched this year:<br />
      <span class="normal-text" v-show="watchedList.length > 0">{{
        watchedList
      }}</span>
    </div>
    <br /><br />
    <div class="flex-row gap selection">
      <div class="multiple-selection">
        <div class="text-header">Multiple Selection</div>
        <MultipleSelection
          :titles="getAllTitles()"
          :currentSelectedItems="watchedList"
          @updateSelectedItems="getUpdatedWatchedList($event)"
        ></MultipleSelection>
      </div>
      <div>
        <div class="text-header">Toggle</div>
        <template v-for="item in movies" v-bind:key="item.id">
          <div class="flex-row center">
            <Toggle
              :id="item.id"
              :title="item.title"
              :watchedList="watchedList"
              @updateToggleValue="getUpdatedWatchedList($event)"
            ></Toggle>
            <label>{{ item.title }}</label>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Lato:ital,wght@0,100;0,300;0,400;0,700;0,900;1,100;1,300;1,400;1,700;1,900&display=swap");
body {
  background: black;
  color: white;
  font-family: "Lato", sans-serif;
  margin-bottom: 100px;
}

.flex-col-end {
  display: flex;
  align-items: end;
}

.space-between {
  justify-content: space-between;
  align-items: start;
}

.toggle {
  display: flex;
}

p {
  font-size: 16px;
  font-weight: 300;
  margin: 0;
}

h3 {
  font-size: 50px;
  font-weight: 700;
  display: flex;
  justify-content: center;
  margin: 40px 0;
  text-transform: capitalize;
  font-style: italic;
}

.grid {
  display: grid;
  grid-gap: 30px;
  margin: 20px;
  grid-template-columns: repeat(auto-fill, 310px);
  justify-content: center;
}

.line {
  border-bottom: 8px solid #222;
  margin: 60px 0;
}

.rating {
  width: fit-content;
}

.flex-col {
  display: flex;
  flex-direction: column;
}

.flex-row {
  display: flex;
  flex-direction: row;
}

.center {
  align-items: center;
}

.text-header {
  font-size: 22px;
  font-weight: 700;
  font-style: italic;
  text-align: center;
}

.normal-text {
  font-size: 18px;
  font-weight: 400;
  font-style: normal;
}

.mrg-20 {
  margin: 20px 0;
}

.gap {
  gap: 50px;
}

.watched-movie {
  height: 50px;
}

.selection .text-header {
  margin-bottom: 20px;
}

.selection .switch {
  margin: 0;
  margin-right: 10px;
  margin-block: 5px;
}

.selection {
  flex-wrap: wrap;
  justify-content: center;
}

.fa {
  color: #ff2c1f;
}
</style>
