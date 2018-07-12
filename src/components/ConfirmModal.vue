<template>
    <div>
        <b-modal v-model="isShow"
                centered
                v-bind:title="title"
                @hidden="initData"
                @ok="handleOk">
            <p>{{message}}</p>
        </b-modal>
    </div>
</template>

<script>
    export default {
        name: "ConfirmModal",
        data () {
            return {
                isShow: false,
                title: undefined,
                message: undefined,
                params: undefined
            }
        },
        methods: {
            initData () {
                this.title = undefined
                this.message = undefined
                this.params = undefined
            },
            handleOk () {
                this.$emit('handleOk', this.params)
            },
            show (title, message, params) {
                this.title = title
                this.message = message
                this.params = params
                this.isShow = !this.isShow
            }
        }
    }
</script>

<style>

</style>
