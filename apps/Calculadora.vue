<script lang="ts">

import {
    Window,
    WinButton
} from "@/components/modules";

export default {
    components: {
        Window,
        WinButton
    },
    data() {
        return {
            visor: 0
        }
    },
    methods: {
        somar() {

        },
        subtrair() {

        },
        multiplicar() {

        },
        dividir() {

        },
        potencia() {

        },
        raiz() {

        }
    }
}

</script>

<template>

<window
    title="Calculadora"
    @on-window-closed='$emit("on-app-closed")'
    @on-window-focus='$emit("on-app-focus")'
    @on-window-minimized='(event) => $emit("on-app-minimized", event)'
>

</window>

</template>

<style lang="scss">

</style>
