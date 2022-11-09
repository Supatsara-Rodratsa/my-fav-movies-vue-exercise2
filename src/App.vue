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

import { ref } from 'vue';
import movies from './assets/movies.json';

const favoriteMovie = ref('');
const watchedList = ref([]);

function renderRating(score) {
  let starItem = [];
  let totalScore = (score*5)/100;
  let fullScore = 5;
  
  while(fullScore > 0) {
    if (Math.trunc(totalScore) > 0) {
      starItem.push('<i class="fa fa-star">');
    } else if (totalScore >= 0.5) {
      starItem.push('<i class="fa fa-star-half-o">');
    } else {
      starItem.push('<i class="fa fa-star-o">');
    }
    totalScore -= 1;
    fullScore--;
  }
  
  return starItem;
}
</script>

<template>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css">
  <h3>Movie list</h3>
  <div class="grid">
    <div v-for="item in movies" v-bind:key="item.id">
      <div class="card flex-col space-between" :style="`background-image: url(${item.picture})`">
        <div class="toggle">
          <label class="switch">
            <input type="checkbox" :id="`option${index}`" :value="item.title" v-model="watchedList">
            <div class="slider round">
             <span class="watched">Watched</span>
             <span class="un-watched">Unwatched</span>
            </div>
          </label>
        </div>
        <div class="content-bg flex-col-end">
          <div class="content-details flex-col">
            <div class="flex-row center space-between">
              <p class="title">{{item.title}}</p>
              <p class="title" v-show="favoriteMovie && favoriteMovie == item.title">😍</p>
            </div>
            <div class="flex-row space-between">
              <div class="flex-row rating">
                <template v-for="score in renderRating(item.score)" :key="score">
                  <div v-html="score" /> 
                </template>
              </div>
              <p>Score: {{item.score}} / 100</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="line"></div>
  <h3>Controls</h3>
  <div class="flex-col center">
    <div class="text-header">My fav movie: <span class="normal-text">&nbsp;{{ favoriteMovie || 'none chosen yet :(' }}</span></div>
    <div class="mrg-20 fav-dropdown ">
      <select v-model="favoriteMovie">
        <option disabled value="">Please select a favorite!</option>
        <template v-for="item in movies" v-bind:key="item.id">
          <option>{{item.title}}</option>
        </template>
      </select>
    </div>
  </div>
  <br><br>
  <div class="flex-col center">
    <div class="text-header watched-movie">Movies watched this year:<br>
      <span class="normal-text" v-show="watchedList.length > 0">{{ watchedList }}</span>
    </div>
    <br><br>
    <div class="flex-row gap selection">
      <div class="multiple-selection">
        <div class="text-header">Multiple Selection</div>
        <select v-model="watchedList" multiple>
          <template v-for="item in movies" v-bind:key="item.id">
            <option>{{item.title}}</option>
          </template>
        </select>
      </div>
      <div>
        <div class="text-header">Toggle</div>
        <template v-for="item in movies" v-bind:key="item.id">
          <div class="flex-row center">
            <label class="switch">
              <input type="checkbox" :id="`option${index}`" :value="item.title" v-model="watchedList">
              <div class="slider round">
               <span class="watched">Watched</span>
               <span class="un-watched">Unwatched</span>
              </div>
            </label>
            <label for="`option${index}`">{{item.title}}</label> 
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Lato:ital,wght@0,100;0,300;0,400;0,700;0,900;1,100;1,300;1,400;1,700;1,900&display=swap');
  body {
    background: black;
    color: white;
    font-family: 'Lato', sans-serif;
    margin-bottom: 100px;
  }
  
  .fa {
    color: #ff2c1f;
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

  .flex-col-end {
    display: flex;
    align-items: end;
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

  .space-between {
    justify-content: space-between;
    align-items: start;
  }

  .title {
    font-size: 24px;
    margin-bottom: 15px;
    max-width: 90%;
    height: 70px;
    display: flex;
    align-items: end;
  }

  .emoji {
    font-size: 18px;
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
    grid-template-columns: 1fr 1fr 1fr 1fr;
    grid-gap: 50px;
    margin: 40px;
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

  .switch {
    position: relative;
    display: inline-block;
    width: 110px;
    height: 34px;
    margin: 20px;
  }
  
  .switch input {
    display:none;
  }
  
  .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #1f1f1f;
    transition: .4s;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
  }
  
  .slider:before {
    position: absolute;
    content: "";
    height: 26px;
    width: 26px;
    left: 4px;
    bottom: 4px;
    background-color: white;
    transition: .4s;
  }
  
  input:checked + .slider {
    background-color: #ff2c1f;
  }
  
  input:checked + .slider:before {
    transform: translateX(75px);
  }
  
  .watched {
    display: none;
  }
  
  .watched, .un-watched {
    color: white;
    position: absolute;
    transform: translate(-50%,-50%);
    top: 50%;
    left: 50%;
    font-size: 12px;
    width: 100%;
    margin-left: 37px;
    font-weight: 500;
  }
  
  input:checked+ .slider .watched {
    display: block;
    text-align: start;
    margin-left: 20px !important;
  }
  
  input:checked + .slider .un-watched {
    display: none;
  }
  
  .slider.round {
    border-radius: 34px;
  }
  
  .slider.round:before {
    border-radius: 50%;
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

  .fav-dropdown select {
    height: 40px;
    border-radius: 30px;
    padding: 0 20px;
    appearance: none;
    background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
    background-repeat: no-repeat;
    background-position: right 1rem center;
    background-size: 1em;
  }

  .multiple-selection select {
    border-radius: 10px;
  }

  .multiple-selection select option { 
    height: 30px;
    display: flex;
    align-items: center;
    padding: 5px 20px;
  }

  select[multiple]:focus option:checked {
    background: lightgray linear-gradient(0deg, lightgray 0%, lightgray 100%) !important;
  }

  select[multiple]:focus {
    border: transparent !important;
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

</style>
