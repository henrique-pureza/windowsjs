<script>

export default {
    name: "TaskbarItem",
    props: {
        shown: Boolean,
        icon: String,
        opened: Boolean,
        label: String,
        target: String
    },
    data() {
        return {
            iconPath: `/_nuxt/assets/icons/${this.icon}.png`
        }
    },
    methods: {
        mouseEnter(event) {
            if (!event.target.classList.contains("shown")) {
                if (event.target.querySelector(".opened")) event.target.querySelector(".opened").style.width = "100%";
            }
        },
        mouseLeave(event) {
            event.target.style.transition = "background-color .1s";

            if (!event.target.classList.contains("shown")) {
                if (event.target.querySelector(".opened")) event.target.querySelector(".opened").style.width = "calc(100% - 5px)";
            }

            setTimeout(() => {
                event.target.style.transition = "";
            }, 100);
        },
        click() {
            this.$emit("click", target);
        }
    }
}

</script>

<template>
    <div
        class="taskbar-item"
        :class="shown && 'shown'"
        :title="label"
        :id="target"
        @mouseenter="mouseEnter"
        @mouseleave="mouseLeave"
        @click="click"
    >
        <img :src="iconPath" class="taskbar-icon" />
        <div class="opened" v-if="opened"></div>
    </div>
</template>
