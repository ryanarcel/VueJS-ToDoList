<template>
    <AddTaskComponent v-if="showAddTask" v-on:add-task="addTask"/>
    <TasksComponent
        v-bind:tasks="tasks" 
        v-on:delete-task="deleteTask"
        v-on:toggle-reminder="toggleReminder"/>
</template>

<script> 
import TasksComponent from '../components/TasksComponent'
import AddTaskComponent from '../components/AddTaskComponent' 


export default {
  name: 'HomeComponent',
  props:{
      showAddTask: Boolean
  },
  components: {
    TasksComponent,
    AddTaskComponent
  },
  data(){
    return{
        tasks: []
    }
  },
  methods:{
    async addTask(task){
      const res = await fetch("http://localhost:3000/tasks",{
        method: 'POST',
        headers:{
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task)
      })

      const data = await res.json()

      this.tasks = [...this.tasks, data]
    },

    async deleteTask(id){
      if(confirm('Are you sure')){
        const res = await fetch(`http://localhost:3000/tasks/${id}`,{
            method: 'DELETE',
        })
        res.status == 200 ?
        this.tasks = this.tasks.filter((task) => task.id !== id): //filters tasks[], returns only data that ids != the param id
        alert('Error deleting task')
      }
    },

    async toggleReminder(id){
      const taskToToggle = await this.fetchTask(id)
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      const res = await fetch(`http://localhost:3000/tasks/${id}`,{
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask)
      })

      const data = await res.json()

      this.tasks = this.tasks.map(
        (task) => task.id === id ?
          {...task, reminder: data.reminder}:
          task
        //maps task array; checks if task id is equal to param id;
        //sets boolean reminder to opposite value;
        // returns task if no task id is equal to param id
      )
    },

     async fetchTasks(){
      const res = await fetch("http://localhost:3000/tasks")

      const data = await res.json()
      return data
    },

    async fetchTask(id){
      const res = await fetch(`http://localhost:3000/tasks/${id}`)

      const data = await res.json()
      return data
    }
  },
  async created(){
    this.tasks = await this.fetchTasks()
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
 