<script setup></script>

<template>
  <ul class="cards text-center">
    <li class="title"><h2>Slide right</h2></li>
    <li class="card card-1">
      <img width="60" height="60" src="@/assets/logo.png" />
      <div class="content">
        <h1>Card 1 Title</h1>
        <p>Card description</p>
      </div>
    </li>
    <li class="card card-2">
      <img src="img/02.jpg" />
      <div class="content">
        <h1>Card 2 Title</h1>
        <p>Card description</p>
      </div>
    </li>
    <li class="card card-3">
      <img src="img/03.jpg" />
      <div class="content">
        <h1>Card 3 Title</h1>
        <p>Card description</p>
      </div>
    </li>
  </ul>
</template>

<style scoped>
ul.cards {
  width: 660px;
  margin: 0 auto 20px;
  height: 300px;
  list-style-type: none;
  position: relative;
  padding: 20px 0;
  cursor: pointer;
}
ul.cards li.title {
  margin: 0 0 20px;
}
ul.cards li.title h2 {
  font-weight: 700;
}
ul.cards li.card {
  background: #fff;
  overflow: hidden;
  height: 200px;
  width: 200px;
  border-radius: 10px;
  position: absolute;
  left: 0px;
  box-shadow: 1px 2px 2px 0 #aaa;
  -webkit-transition: all 0.4s cubic-bezier(0.63, 0.15, 0.03, 1.12);
  transition: all 0.4s cubic-bezier(0.63, 0.15, 0.03, 1.12);
}
ul.cards li.card img {
  max-width: 100%;
  padding: 5px;
  height: auto;
}
ul.cards li.card div.content {
  padding: 5px 10px;
}

ul.cards li.card.card-1 {
  z-index: 10;
  -webkit-transform: rotateZ(-2deg);
  transform: rotateZ(-2deg);
}
ul.cards li.card.card-2 {
  z-index: 9;
  -webkit-transform: rotateZ(-7deg);
  transform: rotateZ(-7deg);
  -webkit-transition-delay: 0.05s;
  transition-delay: 0.05s;
}
ul.cards li.card.card-3 {
  z-index: 8;
  -webkit-transform: rotateZ(5deg);
  transform: rotateZ(5deg);
  -webkit-transition-delay: 0.1s;
  transition-delay: 0.1s;
}

ul.cards:checked li.card {
  -webkit-transform: rotateZ(0deg);
  transform: rotateZ(0deg);
}
ul.cards:checked li.card.card-1 {
  left: 440px;
}
ul.cards:hover li.card.card-2 {
  left: 220px;
}
</style>

<route lang="yml">
meta:
  layout: blank
</route>
