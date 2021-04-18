<template>
    <div :class="[task.reminder ? 'reminder' : '', 'task']" @dblclick="$emit('toggle', task.id)">
        <h3>{{task.text}}
            <i @click="onDelete(task.id)" class="fas fa-times"></i>
        </h3>
        <p>{{task.day}}</p>
    </div>
    
</template>

<script>

export default {
    name: 'Task',
    props:{
        task: Object,
    },
    methods:{
        onDelete(id){
            console.log(id)
            this.$emit('delete-task', id)
        }
    }
}
</script>
<style>

 .task{
  max-width: auto;
  margin: 2px;
  overflow: auto;
  min-height: auto;
  padding: 10px 20px;
  border-radius: 2px;
  justify-content: space-between;
  background: #f4f4f4;
  cursor: pointer;
}

.fas{
    color: red;
}
.task.reminder{
    border-left: 5px solid green;
}

.task h3{
    display: flex;
    align-items: center;
    justify-content: space-between;
}
</style>