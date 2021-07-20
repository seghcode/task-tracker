<template>
    <header>
        <h1>{{ title }}</h1>
        <Button v-show="homePage" @btn-click="$emit('toggle-add-task')" :text="showAddTask ? 'Close' : 'Add Task' "  :color="showAddTask ? 'red' : 'green' "  />
    </header>
</template>

<script>
import Button from "./Button"
export default {
    name: 'Header',
    props: {
        title: String,
        showAddTask: Boolean,
    },
    components: {
        Button,
    },
    computed: {
        homePage(){
            if (this.$route.path === '/') {
                return true
            }else{
                return false
            }
        }
    }
    /*
    advance way of writing properties with default values and defined dtype 
    props: {
        title: {
            type: String,
            default: "Hello World",
        }
    }, */

}
</script>

<style scoped>
    header{
        display: flex;
        justify-content: space-between;
        margin-bottom: 20px;
    }
</style>