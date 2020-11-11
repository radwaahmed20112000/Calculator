<template>
<div>hello, {{ user.firstname + user.secondname }}</div>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {
            followers: 0,
            user: {
                firstname: "Radwa",
                secondname: "Ahmed",
            }
        }
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
