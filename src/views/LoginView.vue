<script setup lang="ts">
import router from '@/router'

type Data = {
  email: string
  password: string
  [key: string]: string
}

function sendData(e: Event) {
  e.preventDefault()

  const obj: Data = { password: '', email: '' }
  const inputs: Array<HTMLInputElement> = [...(e?.target as any)].filter(
    (el) => el?.localName === 'input'
  )

  inputs.forEach((el) => {
    obj[el?.name] = el.value
  })

  console.log(obj)

  //----------------------------------------------------------------------- @TODO: FETCH PRA API!

  router.push({ name: 'tarefas' })
}
</script>

<template>
  <main>
    <div class="loginWrapper">
      <img src="@/assets/logo.svg" width="180" />

      <form class="form" @submit="sendData">
        <div class="inputWrapper">
          <img src="@/assets/mail.svg" width="16" height="16" />
          <input
            name="email"
            class="inputMail"
            type="email"
            placeholder="e-mail"
            autofocus
            required
          />
        </div>

        <div class="inputWrapper">
          <img src="@/assets/pass.svg" width="16" height="16" />
          <div class="passWrapper">
            <input name="password" class="inputPass" type="password" placeholder="senha" required />
            <img src="@/assets/eye.svg" width="16" height="16" />
          </div>
        </div>

        <button class="button" type="submit">Login</button>
      </form>
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
