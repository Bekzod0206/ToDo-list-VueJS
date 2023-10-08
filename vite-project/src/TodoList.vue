<template>
  <div class="todo-list">
    <h2>Tasks List</h2>
    <ul>

      <li v-for="(task, index) in tasks" :key="index"> 
        <div class="todo-text">
          <input class="checkbox-input" type="checkbox" v-model="task.isChecked">
          <span v-if="!task.isEditing" @click="toggleEditing(task)" :class="{'checked-class':task.isChecked}">{{ task.text }}</span>
          <input class="text-input" v-else v-model="task.text" @blur="saveTask(task)" @keyup.enter="saveTask(task)">
        </div>
        <button class="delete-button" @click="deleteItem(index)">&#10006;</button>
      </li>

    </ul>
  </div>
</template>

<script>
export default {
  props:{
    tasks:{
      type: Array,
      required: true
    }
  },
  methods:{

    deleteItem(index) {
      this.$emit('delete-task', index);
      // test-below
      this.saveTasksToLocalStorage();
    },
    toggleEditing(task) {
      task.isEditing = !task.isEditing;
      // test-below
      this.saveTasksToLocalStorage();
    },
    saveTask(task) {
      task.isEditing = false;
      this.$emit('save-task', task);
      // test-below
      this.saveTasksToLocalStorage();
    },
    addTask(task) {
      this.$emit('add-task', task); 
      this.saveTasksToLocalStorage(); 
  },
    saveTasksToLocalStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
  }
}
</script>
<style>
  .todo-list{
	  margin-top: 2rem;
	  padding: 1rem;
	  background-color: #fcfaf5;
	  border-radius: 4px;
	  box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
    font-size: 20px;
  }
  .todo-list li{
    list-style: none;
    display: flex;
    justify-content: space-between;
  }
  .todo-text{
    font-size: 23px;
    margin: 5px 0;
    display: flex;
  }
  .todo-text .checkbox-input{
    height: 20px;
    width: 20px;
    cursor: pointer;
    margin-right: 10px;
  }
  .todo-text .text-input{
    width: 100%;
    height: 25px;
    border: none; 
    font-size: 20px;
    outline: 0.5px solid grey;
  }
  .delete-button{
    cursor: pointer;
    margin: 5px 0;
  }
  .checked-class{
    text-decoration: line-through;
    color: rgb(85, 82, 82);
  }
</style>