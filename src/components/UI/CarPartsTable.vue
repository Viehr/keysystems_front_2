<template>
    <div class="table-container">
      <h2>Добавить запчасть</h2>
      <form @submit.prevent="addPart">
        <div class="form-group">
          <label for="carBrand">Марка автомобиля:</label>
          <input v-model="newPart.carBrand" id="carBrand" type="text" required />
        </div>
        <div class="form-group">
          <label for="country">Страна производства:</label>
          <input v-model="newPart.country" id="country" type="text" required />
        </div>
        <div class="form-group">
          <label for="partName">Название запчасти:</label>
          <input v-model="newPart.partName" id="partName" type="text" required />
        </div>
        <button type="submit">Добавить</button>
      </form>
  
      <table class="parts-table">
        <thead>
          <tr>
            <th @click="sort('carBrand')">Марка автомобиля</th>
            <th @click="sort('country')">Страна производства</th>
            <th @click="sort('partName')">Название запчасти</th>
            <th>Действия</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(part, index) in sortedCarParts" :key="index">
            <td>{{ part.carBrand }}</td>
            <td>{{ part.country }}</td>
            <td>{{ part.partName }}</td>
            <td>
              <button @click="confirmDelete(index)">Удалить</button>
            </td>
          </tr>
        </tbody>
      </table>
  
      <div v-if="showDialog" class="dialog-overlay">
        <div class="dialog-content">
          <p>Вы уверены, что хотите удалить эту запчасть?</p>
          <button @click="deletePart">Да</button>
          <button @click="cancelDelete">Нет</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        carParts: [],
        newPart: {
          carBrand: '',
          country: '',
          partName: '',
        },
        sortKey: '',
        sortAsc: true,
        showDialog: false,
        partToDelete: null,
      };
    },
    computed: {
      sortedCarParts() {
        return [...this.carParts].sort((a, b) => {
          if (this.sortKey) {
            const modifier = this.sortAsc ? 1 : -1;
            if (a[this.sortKey] < b[this.sortKey]) return -1 * modifier;
            if (a[this.sortKey] > b[this.sortKey]) return 1 * modifier;
            return 0;
          }
          return 0;
        });
      },
    },
    methods: {
      addPart() {
        if (
          this.newPart.carBrand.trim() &&
          this.newPart.country.trim() &&
          this.newPart.partName.trim()
        ) {
          this.carParts.push({ ...this.newPart });
          this.newPart.carBrand = '';
          this.newPart.country = '';
          this.newPart.partName = '';
        }
      },
      sort(key) {
        this.sortAsc = this.sortKey === key ? !this.sortAsc : true;
        this.sortKey = key;
      },
      confirmDelete(index) {
        this.showDialog = true;
        this.partToDelete = index;
      },
      deletePart() {
        this.carParts.splice(this.partToDelete, 1);
        this.showDialog = false;
        this.partToDelete = null;
      },
      cancelDelete() {
        this.showDialog = false;
        this.partToDelete = null;
      },
    },
  };
  </script>
  
  <style>
  .table-container {
    width: 100%;
    max-width: 600px;
    margin: 0 auto;
    background-color: #ecf0f1;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  h2 {
    margin-bottom: 20px;
    color: #34495e;
  }
  
  .form-group {
    display: flex;
    flex-direction: column;
    margin-bottom: 15px;
  }
  
  label {
    margin-bottom: 5px;
    font-weight: bold;
    color: #2c3e50;
  }
  
  input[type='text'] {
    padding: 8px;
    border: 1px solid #bdc3c7;
    border-radius: 4px;
    transition: border-color 0.3s ease;
  }
  
  input[type='text']:focus {
    outline: none;
    border-color: #3498db;
  }
  
  button {
    padding: 10px 20px;
    background-color: #3498db;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  button:hover {
    background-color: #2980b9;
  }
  
  .parts-table {
    width: 100%;
    margin-top: 20px;
    border-collapse: collapse;
  }
  
  .parts-table th,
  .parts-table td {
    padding: 12px 15px;
    border: 1px solid #bdc3c7;
  }
  
  .parts-table th {
    background-color: #3498db;
    color: white;
    cursor: pointer;
  }
  
  .parts-table tr:nth-child(even) {
    background-color: #f2f2f2;
  }
  
  .parts-table tr:hover {
    background-color: #e0e0e0;
  }
  
  .dialog-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .dialog-content {
    background: white;
    padding: 20px;
    border-radius: 8px;
    text-align: center;
  }
  
  .dialog-content button {
    margin: 0 10px;
  }
  </style>