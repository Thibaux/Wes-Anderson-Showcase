<template>
    <div id="app">
        <Showcase></Showcase>
    </div>
</template>
<script>
import Showcase from './components/Showcase.vue'

export default {
    name: 'App',
    components: {
        Showcase
    }
}
</script>
<style lang="scss">
@import "./assets/main.scss";

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html,
body {
    margin: 0;
    padding: 0;
    font-family: $font-family;
}

html:focus-within {
    scroll-behavior: smooth;
}

#app {
    font-family: $font-family;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    width: 100vw;
    height: 100vh;
}
</style>