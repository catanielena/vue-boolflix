<template>
    <header>
        <form action="#">
            <input type="text" placeholder="Search" v-model="titleInput">
            <button type="button" @click.prevent="sendTitle">Send</button>
        </form>
    </header>
</template>

<script>
export default {
    name: "Header",
    data() {
        return {
            titleInput: ""
        }
    },
    methods: {
        sendTitle() {
            this.$parent.$emit('titleInput', this.titleInput),
            this.titleInput = ""
        }
    }
}

</script>

<style lang="scss" scoped>

</style>