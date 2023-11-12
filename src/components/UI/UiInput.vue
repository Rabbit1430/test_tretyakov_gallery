<!-- @format -->

<template>
    <input type="text" :value="modelValue" @input="inputUpdate" class="input" />
  </template>
  
  <script>
  export default {
    name: "my-input",
    props: {
      modelValue: [Text],
    },
    methods: {
      inputUpdate(event) {
        this.$emit("update:modelValue", event.target.value);
      },
    },
  };
  </script>
  
  <style scoped>
  .input {
    border: 1px solid #333333;
    padding: 10px 15px;
  }
  </style>
  