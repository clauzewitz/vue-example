<template>
    <div>
        <b-alert :show="dismissCountDown"
            dismissible
            v-bind:variant="variant"
            @dismissed="initData"
            @dismiss-count-down="countDownChanged">
            {{message}}
        </b-alert>
    </div>
</template>

<script>
    export default {
        name: "Alert",
        data () {
            return {
                variant: undefined,
                dismissSec: 3,
                dismissCountDown: 0,
                message: undefined
            }
        },
        methods: {
            initData () {
                this.dismissCountDown = 0
                this.message = undefined
            },
            countDownChanged (dismissCountDown) {
                this.dismissCountDown = dismissCountDown
            },
            show (message, variant) {
                this.variant = variant || 'primary'
                this.message = message
                this.dismissCountDown = this.dismissSec
            }
        },
        TYPE: {
            PRIMARY: 'primary',
            SECONDARY: 'secondary',
            DANGER: 'danger',
            INFO: 'info',
            WARNING: 'warning',
            LIGHT: 'light',
            DARK: 'dark'
        }
    }
</script>

<style>

</style>

