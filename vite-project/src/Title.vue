<template>
  <div class="title-panel">
    <h1>
      Hello, <input type="text" class="input" placeholder="Name here" v-model="name">
    </h1>
    <p>What's on your todo list?</p>
  </div>
</template>
<script>
export default {
  data(){
    return {
      name: ''
    }
  },
  mounted() {
    // Load name from localStorage when the component is mounted
    this.name = localStorage.getItem('name') || '';
  },
  watch:{
    name(newVal){
      localStorage.setItem('name', newVal)
    }
  },
}
</script>
<style>
  .input{
    border: none;
    outline: none;
    font-size: 40px;
    font-weight: 500;
    background-color: #fcfaf5;
    color: grey;
  }
  p{
    font-size: 25px;
  }
</style>