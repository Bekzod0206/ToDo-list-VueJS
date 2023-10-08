<template>
  <div class="app">
    <div class="content">
      <h1 class="app-title">ToDo list</h1>

      <div class="add-input-section">
        <Title />
        <AddPanel @task-added="addTask" />
      </div>

      <div class="search-panel">
        <SearchPanel @search-tasks="updateSearchQuery"/>
        <CategoryPanel @category-selected="updateCategory"/>
      </div>

      <TodoList :tasks="filteredTasks" @delete-task="deleteTask"   @save-task="saveTask" />

    </div>
  </div>
</template>
<script>
import Title from './Title.vue'
import AddPanel from './AddPanel.vue'
import SearchPanel from './SearchPanel.vue'
import CategoryPanel from './CategoryPanel.vue'
import TodoList from './TodoList.vue'
export default {
  components:{
    Title,
    AddPanel,
    SearchPanel,
    CategoryPanel,
    TodoList,
  },
  data(){
    return{
      tasks:[],
      filterType: 'all',
      searchQuery: localStorage.getItem('searchQuery') || '', 
    }
  },
  computed: {
    filteredTasks() {
      let filteredByCategory = this.tasks;
      if (this.filterType === 'completed') {
        filteredByCategory = this.tasks.filter(task => task.isChecked);
      } else if (this.filterType === 'non-completed') {
        filteredByCategory = this.tasks.filter(task => !task.isChecked);
      }
      return filteredByCategory.filter(task =>
        task.text.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },

  },
  methods: {
    addTask(task){
      this.tasks.push(task)

      this.saveTasksToLocalStorage(); 
    },

    saveTasksToLocalStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },

    deleteTask(index) {
      const originalIndex = this.tasks.indexOf(this.filteredTasks[index]);
      if (originalIndex !== -1) {
        this.tasks.splice(originalIndex, 1); 
      }
      this.saveTasksToLocalStorage(); 
    },

    updateCategory(category) {
      this.filterType = category;
    },

    updateSearchQuery(query) {
      this.searchQuery = query;
    },

    saveTask(updatedTask) {
      const index = this.tasks.findIndex(task => task.id === updatedTask.id);
      if (index !== -1) {
        this.$set(this.tasks, index, updatedTask);
      }
    }
  },
  watch: {
    tasks: {
      handler(newTasks) {
        localStorage.setItem('tasks', JSON.stringify(newTasks));
      },
      deep: true 
    },
    searchQuery(newSearchQuery) {
      localStorage.setItem('searchQuery', newSearchQuery);
    },
    filterType(newFilterType) {
      localStorage.setItem('filterType', newFilterType);
    },
  },
  created() {
    const savedTasks = localStorage.getItem('tasks');
    if (savedTasks) {
      this.tasks = JSON.parse(savedTasks);
    }
    const savedFilterType = localStorage.getItem('filterType');
    if (savedFilterType) {
      this.filterType = savedFilterType;
    }
  },
}
</script>
<style scoped>
.app {
	height: 100vh;
	color: #000;
  font-family: 'Roboto', sans-serif;
}
.content {
	width: 1000px;
	min-height: 700px;
	background-color: #fff;
	margin: 0 auto;
	padding: 5rem 0;
}
.add-input-section{
    margin-top: 2rem;
	  padding: 1rem;
	  background-color: #fcfaf5;
	  border-radius: 4px;
	  box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
    font-size: 20px;
}
.search-panel{
    margin-top: 2rem;
	  padding: 1rem;
	  background-color: #fcfaf5;
	  border-radius: 4px;
	  box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
    font-size: 20px;
}
</style>