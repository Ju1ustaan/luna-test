<template>
    <li>
        <div class="wrapp">
            <Arrow 
            v-if="includeChild(list.id)"
            @click="toggle(list.id)" 
            :class="{ 'arrowActive': activeArrow }" />
            <Minus v-else/>
            <p 
            class="title"> 
            {{ list.title }}
            </p>
        </div>
        <div>
            <ul 
            v-if="activeArrow" >
                <ListItem 
                v-for="(sublist) in getSubItems(list.id)" 
                :key="sublist.id" 
                class="child"
                :listOfData="listOfData" 
                :list="sublist"/>
            </ul>
        </div>
    </li>
</template>

<script>
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
    setup({ listOfData }) {
        const openItems = ref([])
        const activeArrow = ref(false)

        const toggle = (id) => {
            if (openItems.value.includes(id)) {
                activeArrow.value = false
                openItems.value = openItems.value.filter(openId => openId !== id)
            } else {
                activeArrow.value = true
                openItems.value = [...openItems.value, id]
            }
        }
        const includeChild = (id) => listOfData.some(i => i.parent_id === id)
        const getSubItems = (id) => listOfData.filter(i => i.parent_id === id)

        return {
            toggle,
            getSubItems,
            activeArrow,
            includeChild
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
