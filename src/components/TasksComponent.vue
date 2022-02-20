<template>
    <div v-bind:key="task.id" v-for="task in tasks">
        <TaskComponent v-on:toggle-reminder="$emit('toggle-reminder', task.id)" v-bind:task="task" v-on:delete-task="$emit('delete-task', task.id)"/>
    </div>
</template>

<script >   
    import TaskComponent from './TaskComponent.vue'

    export default{
        name: 'TasksComponent',
        props: {
            tasks:Array
        },
        components:{
            TaskComponent
        },
       emits:['delete-task', 'toggle-reminder']
    }
</script>
