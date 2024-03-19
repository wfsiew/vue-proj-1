<script setup>
import { ref, computed } from 'vue'

const DEFAULT_STATE = {
  state: true,
  inputName: '',
  names: [],
  error: '',
  showError: false,
  result: ''
}

const data = ref(DEFAULT_STATE)

const isReady = computed(() => {
  return data.value.names.length > 1
})

const addNameToList = () => {
  const userName = data.value.inputName
  if (validate(userName)) {
    data.value.names.push(userName)
    data.value.inputName = ''
    data.value.showError = false
  }
  
  else {
    data.value.showError = true
  }
}

const validate = (value) => {
  data.value.error = ''
  if (value === '') {
    data.value.error = 'Sorry, no empty name'
    return false
  }

  if (data.value.names.includes(value)) {
    data.value.error = 'Sorry, name must be unique'
    return false
  }

  return true
}

const removeName = (index) => {
  data.value.names.splice(index, 1)
}

const getRandomName = () => {
  return data.value.names[Math.floor(Math.random() * data.value.names.length)]
}

const generateResult = () => {
  let rand = getRandomName()
  if (data.value.result !== '') {
    while (rand === data.value.result) {
      rand = getRandomName()
    }
  }
  data.value.result = rand
}

const showResults = () => {
  generateResult()
  data.value.state = false
}

const resetApp = () => {
  data.value.state = true
  data.value.inputName = ''
  data.value.names = []
  data.value.error = ''
  data.value.showError = false
  data.value.result = ''
}

const getNewResult = () => {
  generateResult()
}
</script>

<template>
  <div id="names" class="container" v-if="data.state">
    <div class="logo">
      <h1>Who pays the bill</h1>
    </div>
    <div class="input_container">
      <input type="text" v-model.trim="data.inputName">
      <button @click="addNameToList">Add</button>
    </div>
    <div v-if="data.showError" class="error_label animate__animated animate__fadeInDown">
      {{ data.error }}
    </div>
    <div class="list_of_names">
      <div v-for="(name, index) in data.names" v-bind:key="name" @click="removeName(index)" class="animate__animated animate__fadeIn">
        {{ name }}
      </div>
    </div>
    <div v-if="isReady" class="animate__animated animate__bounceIn">
      <div class="action_button" @click="showResults">
        Check the looser
      </div>
    </div>
  </div>
  <div id="results" class="container" v-if="!data.state">
    <div class="result_container">
      <h1>The looser is:</h1>
      <div class="result_value">
        {{ data.result }}
      </div>
      <div class="action_button" @click="resetApp">
        Start over
      </div>
      <br>
      <div class="action_button btn2" @click="getNewResult">
        Don't like it, get a new name
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
