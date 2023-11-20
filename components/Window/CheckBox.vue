<script lang="ts">

export default {
    name: "CheckBox",
    props: {
        label: String,
        id: String
    }
}

</script>

<template>

<input
    type="checkbox"
    class="check-box"
    :id="id"
/>
<label :for="id">{{ label }}</label>

</template>
