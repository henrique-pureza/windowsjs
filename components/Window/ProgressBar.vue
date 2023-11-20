<script>

export default {
    name: "ProgressBar",
    data() {
        return {
            progressStyle: `width: ${this.completed}%`
        };
    },
    props: {
        completed: String
    }
}

</script>

<template>

<div class="progress-bar">
    <div
        class="progress"
        :style="progressStyle"
    ></div>
</div>

</template>
