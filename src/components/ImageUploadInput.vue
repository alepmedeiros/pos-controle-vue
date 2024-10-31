<template>
    <div class="image-upload-group">
      <label :for="id" class="fixed-label">{{ label }}</label>
      <div class="image-placeholder" @click="triggerFileInput">
        <input type="file" :id="id" @change="handleFileChange" ref="fileInput" />
        <div v-if="!imageSrc" class="placeholder-text">{{ placeholder }}</div>
        <img v-else :src="imageSrc" alt="Product Image" class="uploaded-image"/>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ImageUploadInput',
    props: {
      label: {
        type: String,
        required: true
      },
      id: {
        type: String,
        required: true
      },
      placeholder: {
        type: String,
        default: 'SEM IMAGEM'
      }
    },
    data() {
      return {
        imageSrc: ''
      };
    },
    methods: {
      triggerFileInput() {
        this.$refs.fileInput.click();
      },
      handleFileChange(event) {
        const file = event.target.files[0];
        if (file) {
          const reader = new FileReader();
          reader.onload = (e) => {
            this.imageSrc = e.target.result;
          };
          reader.readAsDataURL(file);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .image-upload-group {
    position: relative;
    height: 100%;
    width: 100%;
  }
  
  .fixed-label {
    position: absolute;
    top: 10px;
    left: 10px;
    font-size: 14px;
    color: #666666;
    background: white;
    padding: 0 5px;
  }
  
  .image-placeholder {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%; /* Ocupa a altura total do container */
    border: 2px solid #3366cc;
    border-radius: 5px;
    background-color: #fff;
    cursor: pointer;
    position: relative;
  }
  
  .image-placeholder input {
    display: none;
  }
  
  .placeholder-text {
    color: #ccc; /* Placeholder text color */
    font-size: 14px;
  }
  
  .image-placeholder img {
    max-width: 100%;
    max-height: 100%;
    border-radius: 5px;
  }

.uploaded-image {
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
  object-fit: contain; /* Ensures the image scales properly within the container */
}
  </style>
  