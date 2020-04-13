<template>
  <div id="app">
    <dice-roller :sides="4" />
    <dice-roller :sides="6" />
    <dice-roller :sides="8" />
    <dice-roller :sides="10" />
    <dice-roller :sides="12" />
    <dice-roller :sides="20" />
  </div>
</template>

<script>
import DiceRoller from './components/DiceRoller.vue';

export default {
  name: 'App',
  components: { DiceRoller }
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  display: grid;
  grid-template-columns: 1fr;
  grid-gap: 1rem;
}
</style>
