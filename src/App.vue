<script setup>

import TodoList from './components/TodoList.vue'



</script>

<template>
  <header>
    <div>
    <todo-list class="box"></todo-list>
    </div>
    <div>


    </div>
  </header>
<body>
</body>

</template>

<style>
@import './assets/base.css';

* {
  box-sizing: border-box;
  margin: auto;
}


header {
  line-height: 1.5;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }



  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
    text-align: center;
    margin-left: 90px;
    margin-bottom: 500px;

  }


}

body {
  background-image: url('Topography.jpg');
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: cover;
}
</style>
