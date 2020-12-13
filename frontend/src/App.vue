<template>
    <div id="app">
        <vue-confirm-dialog></vue-confirm-dialog>
        <NavBar/>
        <router-view />
    </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
export default {
    name: "app",
    components: {
        NavBar,
    }
};
</script>

<style>
</style>