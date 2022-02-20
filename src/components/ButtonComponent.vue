<template>
    <button class="btn"
        v-bind:style={background:color}
        v-on:click="$emit('toggle-add-task')">
        {{text}}
    </button>
</template>

<script>
export default({
    name: 'ButtonComponent',
    props:{
        text:String,
        color:String
    },


    emits:["toggle-add-task"]

})
</script>
