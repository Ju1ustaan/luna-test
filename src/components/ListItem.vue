<template>
    <li>
        <div class="wrapp">
            <Arrow 
            @click="toggle(list.id)" 
            :class="{ 'arrowActive': activeArrow }" />
            <p 
            class="title"> 
            {{ list.title }}
            </p>
        </div>
        <div>
            <ul 
            v-if="isOpen(list.id)" 
            class="list">
                <ListItem 
                v-for="(sublist, idx) in getSubItems(list.id)" 
                :key="sublist.id" class="child"
                :listOfData="listOfData" 
                :list="sublist"
                :class="idx % 2 === 0 ? 'odd' : ''" />
            </ul>
        </div>
    </li>
</template>

<script>
import axios from 'axios'
import { ref } from 'vue'
import ListItem from './ListItem.vue'
import Arrow from './icons/Arrow.vue'
import Minus from './icons/Minus.vue'

export default {
    name: 'list-item',
    components: {
        Arrow,
        Minus
    },
    props: {
        list: Object,
        listOfData: Array,
    },
    setup({ list, listOfData }) {
        const openItems = ref([])
        const activeArrow = ref(false)

        const toggle = (id) => {
            if (openItems.value.includes(id)) {
                activeArrow.value = false
                openItems.value = openItems.value.filter(openId => openId !== id)
            } else {
                activeArrow.value = true
                openItems.value.push(id)
            }
        }

        const isOpen = (id) => openItems.value.includes(id)

        const getSubItems = (parentId) => {
            return listOfData.filter(item => item.parent_id === parentId)
        }
        return {
            toggle,
            isOpen,
            getSubItems,
            activeArrow
        }
    }
}
</script>

<style scoped>
.wrapp {
    padding: 0 20px;
    display: flex;
    align-items: center;
    column-gap: 15px;
    max-height: 41px;
}

.title {
    font-size: 18px;
    max-height: 41px;
    padding: 10px 0;
    font-weight: 500;
    cursor: pointer;

}

.arrowActive {
    transform: rotate(0deg);
}

.child {
    font-size: 16px;
    padding: 0 4em;
    cursor: pointer;
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 150%;
    margin-left: -3.2em;
}

</style>
