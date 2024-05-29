<script setup>
import './assets/main.css'
import axios from 'axios';
import { ref, computed } from 'vue'
import ListItem from './components/ListItem.vue'
import ListButton from './components/ListButton.vue'


const isShow = ref(false)
const listOfData = ref([])

const getData = async () => {
  try {
    const { data } = await axios('https://64b4c8450efb99d862694609.mockapi.io/tree/items')
    listOfData.value = data
  } catch (error) {
    console.error('не получилось', error)
  }
}
const computedList = computed(() => {
  return listOfData.value.filter(i => i.parent_id === null)
})

const showList = () => {
  isShow.value = !isShow.value
  if (isShow.value) getData()
}
</script>

<template>
  <div class="container">
    <div class="wrapper">

      <ListButton @click="showList()" />
      <ul 
      class="list"
      v-if="isShow" >
        <ListItem 
        class="parent"
        v-for="(list, idx) in computedList" 
        :key="list.id" 
        :listOfData="listOfData" 
        :list="list" />
      </ul>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  padding: 20px 0;
  transition: 1s ease-in;

}
.parent {
  background-color: #e4e4e8;
}
.parent:not(:last-child){
  border-bottom: 1px solid #b1b1b3;
}
.list {
  margin-top: 20px;
  border-radius: 5px;
  border: 1px solid #e4e4e8;
  overflow: hidden;
}
</style>
