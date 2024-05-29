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
        <div class="list__bg">
          <div 
          :class="{'odd': idx % 2 === 0}" 
          v-for="(item, idx) in listOfData">
        </div>
        </div>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  padding: 20px 0;
}
.parent {
  position: relative;
  background-color: #e4e4e88a;
  z-index: 99;

}
.list {
  position: relative;
  margin-top: 20px;
  border-radius: 5px;
  border: 1px solid #e4e4e8;
  overflow: hidden;
}

.list__bg {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: 0;
  display: grid;
  grid-template-rows: repeat(53, 41px);
}

.odd {
  background-color: #a5a5a7;
}

</style>
