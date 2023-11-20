<script lang="ts">

export default {
    name: "WinLink",
    props: {
        to: String
    }
}

</script>

<template>

<a :href="to" class="link">
    <slot />
</a>

</template>
