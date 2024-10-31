<template>
    <button :class="buttonClass" @click="onClick">
      <slot></slot>
    </button>
  </template>
  
  <script>
  export default {
    name: 'ActionButton',
    props: {
      buttonClass: {
        type: String,
        default: ''
      },
      onClick: {
        type: Function,
        default: () => {}
      }
    }
  };
  </script>
  
  <style scoped>
  button {
    padding: 10px 20px;
    font-size: 16px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin: 0 10px; /* Espaçamento entre os botões */
    flex: 1; /* Para garantir que os botões cresçam igualmente */
  }
  
  button.cancel {
    background-color: #ff4d4d;
    color: #fff;
  }
  
  button.clear {
    background-color: #ffc107;
    color: #fff;
  }
  
  button.print {
    background-color: #8e44ad;
    color: #fff;
  }
  
  button.pay {
    background-color: #28a745;
    color: #fff;
  }
  </style>
  