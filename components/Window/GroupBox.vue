<script lang="ts">

export default {
    name: "GroupBox",
    props: {
        label: String
    }
};

</script>

<template>
    <div class="group-box">
        <span class="group-box-label">{{ label }}</span>
        <slot />
    </div>
</template>
