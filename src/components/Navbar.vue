<template>
  <nav >
        <h4>Kanban Board</h4>
        <div> 
            <button @click="newTask" type="button" class="btn btn-primary" id="new-task-btn">New Task</button>
            <button @click="logout" type="button" class="btn btn-primary" id="new-task-btn">logout</button>
        <div>
    </nav>
</template>

<script>
export default {
    name: 'Navbar',
    methods: {
        logout() {
            localStorage.clear()
            return this.$emit('logout')
        },
        newTask() {
            return this.$emit('changePage', 'addTask')
        }

    }
}
</script>

<style>

</style>