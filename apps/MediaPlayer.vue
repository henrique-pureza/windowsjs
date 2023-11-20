<script>

import { Window } from "@/components/modules";

export default {
    name: "MediaPlayer",
    components: {
        Window
    }
};

</script>

<template>

<window
    title="Media Player"
    @on-window-closed="$emit('on-app-closed')"
    @on-window-focus="$emit('on-app-focus')"
    @on-window-minimized="(event) => $emit('on-app-minimized', event)"
>

</window>

</template>

<style lang="scss">

</style>
