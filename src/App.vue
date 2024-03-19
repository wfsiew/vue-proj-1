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
  </div>
  <div id="results" class="container" v-if="!data.state"></div>
</template>

<style scoped>

</style>
