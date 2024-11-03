<template>
    <div class="add-transaction">
      <h3>Добавить новую транзакцию</h3>
      <form @submit.prevent="onSubmit">
        <div class="form-control">
          <label for="text">Описание</label>
          <input
            type="text"
            id="text"
            v-model="text"
            placeholder="Введите описание..."
            required
          />
        </div>
        <div class="form-control">
          <label for="amount">
            Сумма <br />
            (положительное значение для дохода, отрицательное для расхода)
          </label>
          <input
            type="number"
            id="amount"
            v-model.number="amount"
            placeholder="Введите сумму..."
            required
          />
        </div>
        <div class="form-control">
          <label for="category">Категория</label>
          <select id="category" v-model="category" required>
            <option disabled value="">Выберите категорию</option>
            <option v-for="cat in categories" :key="cat" :value="cat">{{ cat }}</option>
          </select>
        </div>
        <button class="btn">Добавить транзакцию</button>
      </form>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  
  const props = defineProps({
    categories: {
      type: Array,
      required: true,
    },
  })
  
  const emit = defineEmits(['addTransaction'])
  
  const text = ref('')
  const amount = ref(null)
  const category = ref('')
  
  const onSubmit = () => {
    if (text.value.trim() === '' || amount.value === null || category.value === '') {
      alert('Пожалуйста, заполните все поля.')
      return
    }
  
    const newTransaction = {
      id: Date.now(),
      text: text.value,
      amount: amount.value,
      category: category.value,
      date: new Date().toISOString(),
    }
  
    emit('addTransaction', newTransaction)
  
    text.value = ''
    amount.value = null
    category.value = ''
  }
  </script>
  
  <style scoped>
  .add-transaction {
    margin: 20px 0;
    padding: 10px;
    background: #fff;
    border-radius: 5px;
    box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1);
  }
  
  .add-transaction h3 {
    margin-bottom: 10px;
    animation: fadeInDown 1s ease-out;
  }
  
  .form-control {
    margin-bottom: 10px;
  }
  
  .form-control label {
    display: block;
    margin-bottom: 5px;
  }
  
  .form-control input,
  .form-control select {
    width: 100%;
    padding: 8px;
    box-sizing: border-box;
  }
  
  .btn {
    width: 100%;
    padding: 10px;
    background: #333;
    color: #fff;
    border: none;
    cursor: pointer;
    transition: background 0.3s ease;
  }
  
  .btn, .delete-btn {
  transition: background 0.3s ease, transform 0.3s ease;
}

.btn:hover, .delete-btn:hover {
  transform: scale(1.05);
}

.delete-btn:active {
  transform: scale(0.95);
}

  .btn:hover {
    background: #555;
  }

  </style>
  