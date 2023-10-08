<template>
  <div class="add-panel">
    <input type="text" v-model="newTask" placeholder="Enter your task..." @keyup.enter="addTask" />
    <button @click="addTask">Add Task</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: ''
    }
  },
  methods:{
    addTask(){
      if (this.newTask.trim() !== "") {
        this.$emit("task-added", { text: this.newTask, isChecked: false, isEditing: false });
        this.newTask = "";
      }
    },
  }
}
</script>
<style scoped>
.add-panel {
  display: flex;
  justify-content: space-around;
}
.add-panel input{
  width: 75%;
  height: 30px;
  border: 1px solid grey;
  outline: none;
  border-radius: 5px;
  font-size: 20px;
}
.add-panel button{
  width: 15%;
  cursor: pointer;
  font-size: 15px;
}
</style>