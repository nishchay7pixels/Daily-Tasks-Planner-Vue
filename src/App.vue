<template>
<div class="container">
  <Header title="Task Tracker" @toggle-addTask="toggleAddTask" :showAddTask="showAddTask"/>
  <div v-show="showAddTask">
    <AddTask @add-task="addTask" />
  </div>
  
  <Tasks @delete-task="deleteTask" :tasks="tasks" @toggle="toggle"/>
</div>
</template>

<script>
import AddTask from './components/AddTask.vue'
import Header from './components/Header.vue'
import Tasks from './components/Tasks'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask
  },
  data(){
    return{
      tasks:[],
      showAddTask: false
    }
  },
  async created(){
    this.tasks = await this.fetchTask()
  },
  methods:{
    async deleteTask(id){
      if(confirm('Are you sure?')){
        this.tasks = this.tasks.filter((task) => task.id !==id)
        const res = await fetch(`api/tasks/${id}`,{
          method: 'DELETE',

        })
        res.status ===200? 
        (this.tasks = this.tasks.map((task) => task.id===id?{...task, reminder: !task.reminder}:task)):
        alert("Error Encountered!")
      }
    },
    async toggle(id){
      const taskToToggle = await this.fetchThisTask(id)
      const updatedTask = {...taskToToggle, reminder:!taskToToggle.reminder}
      const res = await fetch(`api/tasks/${id}`,{
        method: 'PUT',
        headers:{
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updatedTask)
      })
      const data = await res.json()
      this.tasks = this.tasks.map((task) => task.id===id?{...task, reminder: data.reminder}:task)
    },
    async addTask(task){
      const res = await fetch('api/tasks', {
        method:'POST',
        headers:{
          'Content-type':'application/json',
          
        },
        body: JSON.stringify(task)
      })
      const data = await res.json()
      this.tasks = [...this.tasks, data]
    },
    toggleAddTask(){
      this.showAddTask = !this.showAddTask
    },
    async fetchTask(){
      const res = await fetch('api/tasks')
      const data = await res.json()
      return data
    },
    async fetchThisTask(id){
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      return data
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body{
  font-family: 'Poppins', sans-serif;
  
}
.container{
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn{
  display: inline-block;
  background: #000;
  color: azure;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
</style>
