<template>
    <div>
        <b-modal v-model="isShow"
                centered
                v-bind:title="title"
                @hidden="initData"
                @ok="handleOk">
            <b-form-input type="text"
                        placeholder="Enter Prohibit word"
                        v-model="prohibitWord">
            </b-form-input>
            <Alert ref="alert"/>
            <p>{{message}}</p>
        </b-modal>
    </div>
</template>

<script>
    import Alert from './Alert'

    export default {
        name: "PromptModal",
        components: {
            Alert
        },
        data () {
            return {
                isShow: false,
                title: undefined,
                message: undefined,
                prohibitWord: undefined
            }
        },
        methods: {
            initData () {
                this.title = undefined
                this.message = undefined
                this.prohibitWord = undefined
            },
            handleOk (event) {
                if (this.prohibitWord && this.prohibitWord.trim()) {
                    this.$emit('handleOk', this.prohibitWord)
                } else {
                    event.preventDefault()
                    this.$refs.alert.show('금칙어를 입력하세요', Alert.TYPE.DANGER)
                }
            },
            show (title, message) {
                this.title = title
                this.message = message
                this.isShow = !this.isShow
            }
        }
    }
</script>

<style>

</style>
