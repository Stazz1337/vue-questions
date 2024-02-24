<script setup>
import { ref, onMounted } from 'vue'

const data = ref({})
const selectedAnswer = ref('')
const showModal = ref(false)

//запрос на сервер

const fetchQuestion = async () => {
  try {
    const response = await fetch('https://ef91880c5e3d6ae5.mokky.dev/question')
    const fetchedData = await response.json()
    data.value = fetchedData[0]
  } catch (error) {
    console.error('Ошибка при получении данных', error)
  }
}

onMounted(fetchQuestion)

// проверка ответа

const checkAnswer = () => {
  if (!selectedAnswer.value) {
    alert('Choose the correct answer')
    return
  } else if (selectedAnswer.value === data.value.correctAnswer) {
    alert('You right!')
  } else {
    alert('You wrong...')
  }
  selectedAnswer.value = ''
}
</script>

<template>
  <div class="container">
    <div class="user">
      <p>Anton</p>
      <div class="balance">
        <p>1000</p>
        <img src="./assets/coin.png" alt="coin" />
      </div>
    </div>

    <div class="question">
      <button class="button"><img src="./assets/close_btn.png" alt="close" /></button>

      <div>
        <p class="title">{{ data.task }}</p>
        <p class="subtitle">{{ data.question }}</p>
      </div>

      <button class="button" @click="showModal = true">
        <img src="./assets/bulb.png" alt="bulb" />
      </button>
    </div>

    <div class="modal" v-if="showModal" @click.self="showModal = false">
      <div class="modal-content">
        <span class="close" @click="showModal = false">&times;</span>
        <p>Choose the correct answer</p>
      </div>
    </div>

    <img class="task-image" :src="data.image" alt="task-image" />

    <div class="radio-section">
      <input
        type="radio"
        name="options"
        v-for="answer in data.answers"
        :key="answer.id"
        :id="answer.id"
        :value="answer.value"
        v-model="selectedAnswer"
      />
      <label
        v-for="answer in data.answers"
        :key="answer.id"
        :for="answer.id"
        :class="{ selected: selectedAnswer === answer.value }"
        >{{ answer.value }}</label
      >

      <button class="checkButton" @click="checkAnswer">Check</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100vh;
  margin: 0 auto;
}

.user {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: end;
  gap: 10px;
  width: 80%;
}

.balance {
  display: flex;
  align-items: center;
  gap: 5px;
}

.question {
  margin: 100px auto 0;
  min-width: 80%;
  display: flex;
  justify-content: space-between;
  align-items: start;
  gap: 20px;
}

.title {
  text-align: center;
}

.subtitle {
  margin-top: 20px;
  font-size: 26px;
  text-align: center;
}

.task-image {
  margin: 100px auto 100px;
  width: 80%;
}

@media screen and (max-width: 768px) {
  .task-image {
    margin: 30px auto 30px;
  }
}

.radio-section {
  background-color: lightgrey;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  gap: 30px;
  width: 100%;
}

@media screen and (max-width: 768px) {
  .radio-section {
    gap: 5px;
  }
}

input[type='radio'] {
  display: none;
}

label {
  padding: 10px 20px;
  background-color: #fff;
  cursor: pointer;
  flex-grow: 1;
  text-align: center;
  border-radius: 5px;
}

label:hover {
  opacity: 0.8;
}

.selected {
  background-color: grey;
}

.checkButton {
  padding: 10px 20px;
  background-color: rgb(19, 114, 223);
  color: white;
  border: none;
  cursor: pointer;
  transition: background-color 0.2s ease;
  border-radius: 5px;
}

.checkButton:hover {
  opacity: 0.8;
}

.button {
  padding: 10px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  box-shadow:
    0 4px 8px 0 rgba(0, 0, 0, 0.2),
    0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
.button:hover {
  opacity: 0.8;
}

.modal {
  background-color: rgba(0, 0, 0, 0.5);
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}

.modal-content {
  display: flex;
  justify-content: center;
  background-color: #fff;
  margin: 10% auto 0;
  padding: 30px;
  width: 300px;
  position: relative;
  border-radius: 5px;
}

.close {
  color: gray;
  font-size: 40px;
  position: absolute;
  top: -3px;
  right: 5px;
}
.close:hover {
  color: black;
  cursor: pointer;
}
</style>
