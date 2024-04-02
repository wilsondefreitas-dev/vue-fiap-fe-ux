<script setup lang="ts">
import { RouterLink } from 'vue-router'
</script>

<template>
  <main>
    <div class="loginWrapper">
      <img src="@/assets/logo.svg" width="180" />

      <div class="form">
        <div class="inputWrapper">
          <img src="@/assets/mail.svg" width="16" height="16" />
          <input class="inputMail" type="email" placeholder="e-mail" />
        </div>

        <div class="inputWrapper">
          <img src="@/assets/pass.svg" width="16" height="16" />
          <div class="passWrapper">
            <input class="inputPass" type="password" placeholder="senha" />
            <img src="@/assets/eye.svg" width="16" height="16" />
          </div>
        </div>
      </div>

      <RouterLink class="linkButton" to="/tarefas">
        <button class="button" type="submit">Login</button>
      </RouterLink>
    </div>
  </main>
</template>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.loginWrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 32px;
}

.form {
  display: flex;
  gap: 15px;
  flex-direction: column;
}

.inputWrapper {
  display: flex;
  align-items: center;
  gap: 8px;
}

.linkButton {
  width: 100%;
}

.button {
  background-color: #ed1164;
  border-radius: 12px;
  padding: 15px;
  width: 100%;
  border: none;
  color: white;
  font-size: 16px;
  font-weight: 600;
}

input {
  border: none;
}

.inputMail {
  width: 224px;
  border-bottom: solid 1px #ed1164;
}

.passWrapper {
  border-bottom: solid 1px #ed1164;
}

.inputPass {
  width: 208px;
}
</style>
