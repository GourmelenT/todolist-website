<script setup>
import AddCard from './AddCard.vue'
</script>

<script>
export default {
  methods: {
    transfertNewTask(newTask) {
      this.$emit('newTask', newTask)
    },
  }
}
</script>

<template>
  <div class="app-infos-container">
    <img alt="Vue logo" class="logo" src="../assets/logo.png" width="150" height="150" />
    <div class="title">
      <h1 class="primary-color">Check It ✓</h1>
      <h3>Une TO-DO list sur mesure!</h3>
    </div>
  </div>
  <div class="add-todo-container">
    <AddCard @newTask="transfertNewTask" />
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.app-infos-container {
  display: flex;
}

.add-todo-container {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  margin-top: 3rem;
}

.title h1,
.title h3 {
  text-align: center;
}

@media (max-width: 1024px) {
  .app-infos-container{
    flex-direction: row;
    align-items: center;
    justify-content: center;
  }
}
</style>
