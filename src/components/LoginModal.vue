<template>
  <div class="modal" v-if="isVisible">
    <div class="modal-content">
      <span class="close" @click="closeModal">&times;</span>
      <h2>Entrar</h2>
      <form @submit.prevent="login">
        <input type="email" placeholder="Email" required />
        <input type="password" placeholder="Senha" required />
        <button type="submit">ENTRAR</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "LoginModal",
  data() {
    return {
      isVisible: false,
    };
  },
  methods: {
    closeModal() {
      this.isVisible = false;
    },
    login() {
      // Lógica de login
    },
  },
};
</script>

<style scoped>
.modal {
  display: flex;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 5px;
  width: 300px; 
  text-align: left;
}

.close {
  float: right;
  font-size: 28px;
  cursor: pointer;
}

button {
  background-color: rgba(65, 22, 127); 
  color: white; 
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin-left: 30px; 
}

input[type="email"],
input[type="password"] {
  border: 1px solid rgba(65, 22, 127); 
  margin-bottom: 10px;
}

a {
  color: rgba(65, 22, 127);
  cursor: pointer;
}
</style>
