<template>
    <div class="TaskList" :key="task.id" v-for="task in tasks">
        <Task @delete-task="$emit('delete-task', task.id)" :task="task" @toggle="$emit('toggle',task.id)"/>
    </div>
</template>
<script>
import Task from './Task'

export default {
    name: 'Tasks',
    props: {
        tasks: Array,
    },
    components:{
        Task,
    },
    emit:['delete-task']
}
</script>
<style scoped>

</style>