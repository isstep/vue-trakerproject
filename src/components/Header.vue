<template>
    <header>
      <h2>Финансовый Трекер</h2>
    </header>
  </template>
  
  <script setup>
  </script>
  
  <style scoped>
  header {
    background: #333;
    color: #fff;
    padding: 10px 0;
    text-align: center;
    margin-bottom: 20px;
  }
  
  h2 {
    margin: 0;
    font-size: 24px;
    animation: fadeInDown 1s ease-out;
  }
  
  @keyframes fadeInDown {
    from {
      opacity: 0;
      transform: translateY(-20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  </style>