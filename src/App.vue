<template>
    <div id="app" class="container">
        <Prohibit/>
    </div>
</template>

<script>
    import Prohibit from './components/Prohibit'

    export default {
        name: 'App',
        components: {
            Prohibit
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
