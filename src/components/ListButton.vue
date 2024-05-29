<template>
    <div class="list__button">
        <button>Открыть список</button>
    </div>
</template>

<script>
export default {
    name: 'list-button'
}
</script>

<style scoped>
.list__button {
    display: flex;
    justify-content: center;
}

.list__button button {
    text-align: center;
    padding: 20px;
    border-radius: 5px;
    background-color: #e4e4e8;
}
</style>