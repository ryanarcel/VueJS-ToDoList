<template>
    <header>
        <h1>{{title}}</h1>
        <ButtonComponent
             v-on:toggle-add-task="$emit('toggle-add-task')"
             v-bind:text="btnLabel ? 'Close': 'Add Task'"
             v-bind:color="btnLabel ? 'red': 'green'"
             v-show="homePage"
        />
    </header>
</template>

<script>
import ButtonComponent from './ButtonComponent'

export default{
    name:'HeaderComponent',
    props:{
        title: String,
        btnLabel: Boolean
    },
    components:{
        ButtonComponent
    },
    computed:{
        homePage(){
            if(this.$route.path ==="/"){
                return true
            }
            else{
                return false
            }
        }
    },
    emits: ['toggle-add-task']
}
</script>

<style scoped>
header{
    display:flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}
</style>