<script setup lang="ts">
import { onMounted, ref, type Ref } from 'vue'
import { RouterLink } from 'vue-router'

//

type TaskObject = {
  [key: string]: string | null | undefined
  id?: string
  name: string
  date: string
  conclusion_date: string | null
}

//

const showModal = ref(false)
const tasks: Ref<TaskObject[]> = ref([])

//

onMounted(() => {
  fetchTasks()
})

//

function handleSubmit(e: Event) {
  const obj: TaskObject = { name: '', date: '', conclusion_date: null }
  const inputs: Array<HTMLInputElement> = [...(e?.target as any)].filter(
    (el) => el?.localName === 'input'
  )

  inputs.forEach((el) => {
    obj[el?.name] = el.value
  })

  addTask(obj?.name, obj?.date)
  showModal.value = false
}

async function addTask(name: string, deadline: string) {
  const success = await createTask(name, deadline)

  if (!success) throw new Error('Something went wrong! Try again later...')

  tasks.value.push({
    name: name,
    date: deadline,
    conclusion_date: null
  })
}

async function completeATask(taskId: string | undefined) {
  const taskIndex = tasks.value.findIndex((task) => task?.id === taskId)
  const curDate = new Date().toISOString().slice(0, 19).replace('T', ' ').split(' ')[0]
  const valueToAdd = tasks.value[taskIndex].conclusion_date ? null : curDate

  const success = updateTaskConclusionDate(valueToAdd)

  if (!success) throw new Error('Something went wrong! Try again later...')

  tasks.value[taskIndex].conclusion_date = valueToAdd
}

function fetchTasks() {
  //----------------------------------------------------------------------- @TODO: FETCH PRA API PARA PEGAR TAREFAS!
  tasks.value = [
    { id: '1aD9', name: 'Atividade 1', date: '2024-05-20', conclusion_date: null },
    { id: '2Dko', name: 'Atividade 2', date: '2024-06-13', conclusion_date: '2024-06-10' },
    { id: '9jD8', name: 'Atividade 3', date: '2024-04-30', conclusion_date: null }
  ]
}

function createTask(name: string, deadline: string) {
  console.log(name, deadline)
  //----------------------------------------------------------------------- @TODO: POST PARA ADICIONAR TAREFAS!
  return true
}

function updateTaskConclusionDate(conclusionDate: string | null) {
  console.log(conclusionDate)
  //----------------------------------------------------------------------- @TODO: POST PARA ATUALIZAR TAREFAS!
  return true
}
</script>

<template>
  <!-- MODAL -->
  <div class="modalWrapper" v-if="showModal">
    <form class="modal" @submit="handleSubmit">
      <h3>Adicionar uma tarefa</h3>
      <input name="name" type="text" placeholder="Adicionar uma tarefa" autofocus required />
      <input name="date" type="date" placeholder="Data de conclusão" required />

      <div class="buttonsWrapper">
        <button type="submit">Salvar</button>
        <button @click="showModal = false">Cancelar</button>
      </div>
    </form>
  </div>
  <!--  -->

  <header>
    <img src="@/assets/logo.svg" width="89" height="24px" />
    <div class="rightWrapper">
      <span>Olá, Rafael</span>
      <RouterLink class="linkButton" to="/">
        <img src="@/assets/quit.svg" width="24" />
      </RouterLink>
    </div>
  </header>

  <main>
    <div class="blockButton taskFilter">
      <span>Tarefas</span>
    </div>

    <div v-if="tasks.length === 0" class="mainView">
      <div class="noData">
        <img src="@/assets/noData.svg" width="248" height="80" />
        <div>Você ainda não possui tarefas cadastradas!</div>
      </div>
    </div>

    <div v-if="tasks.length > 0">
      <div
        class="blockButton taskButton"
        name="task.id"
        v-for="task in tasks"
        :key="task.name"
        @click="completeATask(task.id)"
      >
        <div class="task">
          <div class="checked">
            <img v-if="!task.conclusion_date" src="@/assets/round.svg" width="20" />
            <img v-if="task.conclusion_date" src="@/assets/done.svg" width="20" />
          </div>
          <div class="data">
            <div>
              <s v-if="task.conclusion_date"
                ><b>{{ task.name }}</b></s
              >
              <b v-if="!task.conclusion_date">{{ task.name }}</b>
            </div>
            <div>Conclusão em: {{ task.date }}</div>
          </div>
        </div>
      </div>
    </div>

    <div class="blockButton">
      <div class="addTask">
        <img src="@/assets/roundAdd.svg" width="24" />
        <span @click="showModal = true">Adicionar uma tarefa</span>
      </div>
    </div>
  </main>
</template>

<style>
header,
.blockButton {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 30px;

  height: 72px;
  border: 1px solid #91a3ad26;
}

header > .rightWrapper {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #ed1164;
  font-size: 12px;
}

main {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  padding: 18px 12px;
}

.blockButton {
  font-size: 18px;
  height: 48px;
  border-radius: 12px;
  color: #ed1164;
  background-color: #91a3ad26;
  font-weight: 600;
  border: none;
}

.blockButton:last-child {
  margin-top: auto;
}

.taskFilter {
  margin-bottom: 8px;
}

.addTask {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #ed1164;
  font-size: 18px;
  font-weight: 600;
}

.mainView {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  align-items: center;
}

.noData {
  display: flex;
  flex-direction: column;
  margin: auto;
  align-items: center;
  gap: 12px;
}

.noData > div {
  text-align: center;
  color: #ed1164;
  font-weight: 600;
  font-size: 14px;
}
</style>

<!-- modal -->
<style>
.modalWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.8);
  position: absolute;
  padding: 60px;
}

.modal {
  display: flex;
  flex-direction: column;
  border-radius: 12px;
  background-color: white;
  align-items: center;
  padding: 24px 12px;
}

.modal > h3 {
  display: flex;
  flex-direction: column;
  color: #ed1164;
  text-align: center;
  margin: 0;
  padding-bottom: 18px;
}

.modal > input {
  color: #ed1164;
  border: solid 1px #ed1164;
  border-radius: 12px;
  padding: 12px;
  width: 287px;
  font-size: 14px;
  margin-bottom: 8px;
}

.modal > .buttonsWrapper {
  display: flex;
  gap: 6px;
  padding-top: 18px;
}

.modal > .buttonsWrapper > button:first-child {
  padding: 12px 28px;
  color: white;
  background-color: #91a3ad;
  border: none;
  border-radius: 12px;
}

.modal > .buttonsWrapper > button:last-child {
  padding: 12px 28px;
  color: #91a3ad;
  border: none;
  border-radius: 12px;
  background-color: transparent;
  text-decoration: underline;
}
</style>

<!-- task -->
<style>
.taskButton {
  margin-top: 8px !important;
}

.task {
  display: flex;
  gap: 12px;
  color: #575757;
  align-items: center;
}

.task > .data > div:first-child {
  font-size: 14px;
  font-weight: 500;
  margin-bottom: 6px;
}

.task > .data > div:last-child {
  font-size: 12px;
  font-weight: 400;
}
</style>
