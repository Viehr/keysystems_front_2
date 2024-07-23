<template>
    <div id="app" class="app-container">
      <app-header />
      <main class="main-content">
        <car-parts-table />
      </main>
    </div>
  </template>
  
  <script>
  import AppHeader from './AppHeader.vue';
  import CarPartsTable from './CarPartsTable.vue';  
  
  export default {
    name: 'AppContainer',
    components: {
      AppHeader,
      CarPartsTable,
    },
  };
  </script>
  
  <style>
  .app-container {
    font-family: 'Arial', sans-serif;
    text-align: center;
    color: #2c3e50;
    padding: 20px;
  }
  
  .main-content {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    min-height: 70vh;
  }
  </style>