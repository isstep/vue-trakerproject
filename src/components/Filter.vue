    <script setup>
  import { ref } from 'vue'
  
  const props = defineProps({
    categories: {
      type: Array,
      required: true,
    },
  })
  
  const emit = defineEmits(['filter', 'search'])
  
  const selectedCategory = ref('Все')
  const searchQuery = ref('')
  
  const onFilter = () => {
    emit('filter', selectedCategory.value)
  }
  
  const onSearch = () => {
    emit('search', searchQuery.value)
  }
  </script>
  
  <template>
    <div class="filter">
      <label for="category-filter">Фильтр по категории:</label>
      <select id="category-filter" v-model="selectedCategory" @change="onFilter">
        <option value="Все">Все</option>
        <option v-for="cat in categories" :key="cat" :value="cat">{{ cat }}</option>
      </select>
      <input
        type="text"
        v-model="searchQuery"
        @input="onSearch"
        placeholder="Поиск по описанию..."
        class="search-input"
      />
    </div>
  </template>
  
  <style scoped>
  .filter {
    margin: 20px 0;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  
  .filter label {
    margin-right: 10px;
  }
  
  .filter select {
    padding: 5px;
  }
  
  .search-input {
    flex: 1;
    padding: 5px;
  }
  </style>