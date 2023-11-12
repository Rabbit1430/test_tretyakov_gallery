<!-- @format -->

<template>
    <button class="btn">
      <slot></slot>
    </button>
  </template>
  
  <script>
  export default {
    name: "ui-button",
  };
  </script>
  
  <style scoped>
  .btn {
    padding: 10px 30px;
    background: none;
    color: #333333;
    border: 1px solid #333333;
    cursor: pointer;
  }
  </style>
  