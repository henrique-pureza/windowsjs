<script>

export default {
    name: "StartMenuApp",
    props: {
        label: String,
        icon: String,
        withAppGroup: String
    },
    data() {
        return {
            imgPath: `/_nuxt/assets/icons/${this.icon}.png`
        }
    },
    methods: {
        mouseDown(event) {
            event.target.style.transform = "scale(95%)";
        },
        mouseUp(event) {
            event.target.style.transform = "";
        },
        click() {
            this.$emit("click");
        }
    }
};

</script>

<template>
    <div class="app-group" v-if="withAppGroup">{{ withAppGroup }}</div>
    <div class="app" @mousedown="mouseDown" @mouseup="mouseUp" @click="click">
        <div class="app-child">
            <img :src="imgPath" />
            {{ label }}
        </div>
    </div>
</template>
