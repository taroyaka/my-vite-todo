<script setup>
import TheHeader from './components/TheHeader.vue';
import TheFooter from './components/TheFooter.vue';
import MainToDo from './components/MainToDo.vue';
</script>


<template>
  <div class="wrap">
      <TheHeader />
      <MainToDo />
      <TheFooter />
  </div>
</template>

<style>
@import 'assets\css/destyle.css';
.wrap{
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 100vh;
  width: 370px;
  margin: 0 auto;
  font-family: sans-serif;
}
.main{
  flex: 1;
  width: 100%;
}
</style>
