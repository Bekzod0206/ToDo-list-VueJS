<template>
  <div class="category-block">
      <button :class="{ 'active': selectedCategory === 'all' }"  @click="selectCategory('all')">All</button>
      <button :class="{ 'active': selectedCategory === 'completed' }"  @click="selectCategory('completed')">Completed</button>
      <button :class="{ 'active': selectedCategory === 'non-completed' }"  @click="selectCategory('non-completed')">Non-Completed</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      selectedCategory: localStorage.getItem('selectedCategory') || 'all', 
    }
  },
  methods: {
    selectCategory(category) {
      this.selectedCategory = category
      this.$emit('category-selected', category);
      // test-below
      localStorage.setItem('selectedCategory', category); 
    },
  }
}
</script>
<style scoped>
  .active {
    background-color: rgb(92, 92, 98);
    color: #fcfaf5;
  }
  .category-block{
    margin-top: 20px;
    display: flex;
    align-items: center;
  }
  .category-block button{
    width: 120px;
    height: 30px;
    margin-right: 10px;
    font-size: 15px;
    cursor: pointer;
    border-radius: 5px;
    border: none;
    transition: background-color 0.7s ease;
  }
</style>