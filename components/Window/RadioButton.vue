<script>

export default {
    name: "RadioButton",
    props: {
        id: String,
        label: String,
        name: String,
        checked: Boolean
    }
}

</script>

<template>

<input
    type="radio"
    :id="id"
    :name="name"
    class="radio-button"
    :checked="checked"
/>
<label :for="id">{{ label }}</label>

</template>
