<template>
  <nav class="navbar">
    <div class="navbar-left">
      <button class="menu-button" @click="toggleMenu">☰</button>
      <span class="navbar-title">SISTEMA DE NFC-e</span>
    </div>
    <div class="navbar-right">
      <img class="user-avatar" src="https://avatar.iran.liara.run/public/30" alt="User Avatar" />
      <span class="user-name">Alessandro Medeiros</span>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'AppNavbar',
  methods: {
    toggleMenu() {
      console.log('Menu button clicked');
    }
  }
};
</script>

<style scoped>
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #3366cc;
  color: white;
  padding: 10px 20px;
  position: fixed;
  top: 0;
  left: 0;  /* Adicionado */
  right: 0;  /* Adicionado */
  width: 100%;
  z-index: 1000;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  box-sizing: border-box;
}

.navbar-left {
  display: flex;
  align-items: center;
}

.menu-button {
  background: none;
  border: none;
  color: white;
  font-size: 24px;
  cursor: pointer;
}

.navbar-title {
  margin-left: 10px;
  font-size: 20px;
  font-weight: bold;
}

.navbar-right {
  display: flex;
  align-items: center;
}

.user-avatar {
  width: 40px; /* Tamanho fixo do avatar */
  height: 40px; /* Tamanho fixo do avatar */
  border-radius: 50%;
  margin-right: 10px;
  object-fit: cover; /* Para garantir que a imagem se adapte ao tamanho */
}

.user-name {
  font-size: 16px;
}

@media (max-width: 600px) {
  .navbar {
    flex-direction: column;
    align-items: flex-start;
  }
  .navbar-left, .navbar-right {
    width: 100%;
    justify-content: space-between;
    margin: 5px 0;
  }
  .menu-button {
    font-size: 20px;
  }
  .navbar-title {
    font-size: 18px;
  }
  .user-name {
    font-size: 14px;
  }
}
</style>
