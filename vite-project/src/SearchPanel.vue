<template>
  <div class="search-block">
    <h2>Search panel and Filter</h2>
    <div class="search-panel">
      <input type="text" placeholder="Search for your tasks..." v-model="searchQuery" @input="searchTasks">
      <button class="delete-button" @click="clearSearch">&#10006;</button>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      searchQuery: localStorage.getItem('searchQuery') || '',
    }
  },
  methods: {
    searchTasks() {
      this.$emit('search-tasks', this.searchQuery);
      localStorage.setItem('searchQuery', this.searchQuery);
    },
    clearSearch() {
      this.searchQuery = ''; 
      this.searchTasks();
    }
  } 
}
</script>
<style scoped>
  .search-panel{
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 auto;
  }
  .search-panel input{
    width: 90%;
    height: 30px;
    border: 1px solid grey;
    outline: none;
    font-size: 20px;
    border-radius: 5px;
  }
  .delete-button{
    height: 34px;
    width: 34px;
  }
</style>