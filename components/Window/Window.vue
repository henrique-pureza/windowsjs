<script>

import maximizeIcon from "@/assets/icons/maximizar.png";
import restoreIcon from "@/assets/icons/restaurar.png";

export default {
    name: "Window",
    props: {
        title: String,
        statusBarText: String,
        statusBar: Boolean
    },
    data() {
        return {
            pos1: 0,
            pos2: 0,
            pos3: 0,
            pos4: 0,
            oldTop: null,
            oldLeft: null,
            defaultPos: true,
            maximized: false,
            closed: false,
            bordersHidden: false,
            progressBars: null,
            isMovable: true
        };
    },
    mounted() {
        if (this.$el.querySelectorAll(".progress")) {
            this.progressBars = this.$el.querySelectorAll(".progress");

            this.progressBarAnimation();
            setInterval(() => {
                this.progressBarAnimation();
            }, 2000);
        }
    },
    methods: {
        maximizeWindow() {
            const windowContainer = this.$refs.windowContainer;

            if (!this.maximized) {
                this.maximized = true;
                this.isMovable = false;

                if (windowContainer.classList.contains("default-pos")) {
                    windowContainer.classList.remove("default-pos");

                    this.oldTop = "75px";
                    this.oldLeft = "75px";
                } else {
                    this.oldTop = getComputedStyle(windowContainer).top;
                    this.oldLeft = getComputedStyle(windowContainer).left;
                }

                this.$refs.maximize.style.backgroundImage = `url("${restoreIcon}")`;
                windowContainer.style.display = "block";
                this.bordersHidden = true;

                this.$refs.window.style.width = "100%";
                this.$refs.window.style.height = "calc(100% - 40px)";

                if (this.defaultPos) {
                    windowContainer.style.right = "5px";
                    windowContainer.style.bottom = "5px";
                    windowContainer.style.width = "95%";
                    windowContainer.style.height = "95%";
                    windowContainer.style.animation = "maximize .2s";
                } else {
                    windowContainer.style.top = "";
                    windowContainer.style.left = "";
                    windowContainer.style.right = "5px";
                    windowContainer.style.bottom = "5px";
                    windowContainer.style.width = "95%";
                    windowContainer.style.height = "95%";
                    windowContainer.style.animation = "maximize .2s";
                }

                setTimeout(() => {
                    windowContainer.style.width = "100%";
                    windowContainer.style.height = "100%";
                    windowContainer.style.right = "";
                    windowContainer.style.bottom = "";
                    windowContainer.style.top = 0;
                    windowContainer.style.left = 0;
                }, 150);
            } else {
                this.maximized = false;
                this.isMovable = true;

                if (this.oldTop !== "75px" && this.oldLeft !== "75px") this.defaultPos = false;

                this.$refs.maximize.style.backgroundImage = `url("${maximizeIcon}")`;

                if (this.defaultPos) {
                    windowContainer.classList.add("default-pos");
                    windowContainer.style.width = "461px";
                    windowContainer.style.height = "361px";
                    windowContainer.style.animation = "restore .2s";
                    windowContainer.style.right = "";
                    windowContainer.style.bottom = "";
                    windowContainer.style.top = "";
                    windowContainer.style.left = "";
                    windowContainer.style.transformOrigin = "bottom right";
                } else {
                    windowContainer.style.width = "461px";
                    windowContainer.style.height = "361px";
                    windowContainer.style.animation = "restore .2s";
                    windowContainer.style.right = "";
                    windowContainer.style.bottom = "";
                    windowContainer.style.transformOrigin = "bottom right";
                    windowContainer.style.top = this.oldTop;
                    windowContainer.style.left = this.oldLeft;
                }

                this.$refs.window.style.width = "calc(100% - 13px)";
                this.$refs.window.style.height = "calc(100% - 13px)";
                windowContainer.style.display = "flex";
                this.bordersHidden = false;

                setTimeout(() => {
                    windowContainer.style.transformOrigin = "";
                }, 200);
            }
        },
        closeWindow() {
            const windowContainer = this.$refs.windowContainer;

            windowContainer.style.animation = "close .2s";
            setTimeout(() => {
                this.closed = true;
                this.$emit("on-window-closed");
            }, 150);
        },
        moveWindow(event) {
            event.preventDefault();

            if (this.isMovable) {
                this.pos3 = event.clientX;
                this.pos4 = event.clientY;

                document.onmouseup = () => {
                    document.onmouseup = e => {
                        const li = document.querySelectorAll(".menu-option");
                        const dropdown = document.querySelectorAll(".dropdown");

                        li.forEach((el, i) => {
                            if (!el.contains(e.target)) {
                                dropdown[i].style.display = "none";
                                el.classList.remove("hover", "active");

                                document.querySelectorAll(".menu-option").forEach(el => el.classList.remove("clicked"));
                            }
                        });
                    }

                    document.onmousemove = null;
                }

                document.onmousemove = e => {
                    e.preventDefault();

                    const windowContainer = this.$refs.windowContainer;

                    if (this.defaultPos) {
                        this.defaultPos = false;

                        windowContainer.classList.remove("default-pos");
                        windowContainer.style.top = "75px";
                        windowContainer.style.left = "75px";
                    }

                    this.pos1 = this.pos3 - e.clientX;
                    this.pos2 = this.pos4 - e.clientY;
                    this.pos3 = e.clientX;
                    this.pos4 = e.clientY;

                    windowContainer.style.top = (windowContainer.offsetTop - this.pos2) + "px";
                    windowContainer.style.left = (windowContainer.offsetLeft - this.pos1) + "px";
                }
            }
        },
        resize(event, direction) {
            event.preventDefault();

            const windowContainer = this.$refs.windowContainer;

            switch (direction) {
                case "up":
                    document.onmousemove = e => {
                        const height = windowContainer.offsetHeight;
                        const top = windowContainer.offsetTop;

                        const newHeight = height - (e.clientY - top);
                        const newTop = top + (height - newHeight);

                        windowContainer.style.height = newHeight + "px";
                        windowContainer.style.top = newTop + "px";
                    }
                    break;
                case "down":
                    document.onmousemove = e => {
                        const height = windowContainer.offsetHeight;
                        const top = windowContainer.offsetTop;

                        const newHeight = (((height + e.clientY) - height) - top) + "px";
                        windowContainer.style.height = newHeight;
                    }
                    break;
                case "left":
                    document.onmousemove = e => {
                        const width = windowContainer.offsetWidth;
                        const left = windowContainer.offsetLeft;

                        const newWidth = width - (e.clientX - left);
                        const newLeft = left + (width - newWidth);

                        windowContainer.style.width = newWidth + "px";
                        windowContainer.style.left = newLeft + "px";
                    }
                    break;
                case "right":
                    document.onmousemove = e => {
                        const width = windowContainer.offsetWidth;
                        const left = windowContainer.offsetLeft;

                        const newWidth = (((width + e.clientX) - width) - left) + "px";
                        windowContainer.style.width = newWidth;
                    }
                    break;
                case "top-left":
                    document.onmousemove = e => {
                        const width = windowContainer.offsetWidth;
                        const height = windowContainer.offsetHeight;
                        const left = windowContainer.offsetLeft;
                        const top = windowContainer.offsetTop;

                        const newWidth = width - (e.clientX - left);
                        const newHeight = height - (e.clientY - top);
                        const newLeft = left + (width - newWidth);
                        const newTop = top + (height - newHeight);

                        windowContainer.style.width = newWidth + "px";
                        windowContainer.style.height = newHeight + "px";
                        windowContainer.style.left = newLeft + "px";
                        windowContainer.style.top = newTop + "px";
                    }
                    break;
                case "top-right":
                    document.onmousemove = e => {
                        const width = windowContainer.offsetWidth;
                        const height = windowContainer.offsetHeight;
                        const top = windowContainer.offsetTop;
                        const left = windowContainer.offsetLeft;

                        const newWidth = ((width + e.clientX) - width) - left;
                        const newHeight = height - (e.clientY - top);
                        const newTop = top + height - (height - (e.clientY - top));

                        windowContainer.style.width = newWidth + "px";
                        windowContainer.style.height = newHeight + "px";
                        windowContainer.style.top = newTop + "px";
                    }
                    break;
                case "bottom-left":
                    document.onmousemove = e => {
                        const width = windowContainer.offsetWidth;
                        const left = windowContainer.offsetLeft;
                        const height = windowContainer.offsetHeight;
                        const top = windowContainer.offsetTop;

                        const newWidth = width - (e.clientX - left);
                        const newHeight = ((height + e.clientY) - height) - top;
                        const newLeft = left + (width - (width - (e.clientX - left)));

                        windowContainer.style.width = newWidth + "px";
                        windowContainer.style.height = newHeight + "px";
                        windowContainer.style.left = newLeft + "px";
                    }
                    break;
                case "bottom-right":
                    document.onmousemove = e => {
                        const width = windowContainer.offsetWidth;
                        const height = windowContainer.offsetHeight;
                        const left = windowContainer.offsetLeft;
                        const top = windowContainer.offsetTop;

                        const newWidth = ((width + e.clientX) - width) - left;
                        const newHeight = ((height + e.clientY) - height) - top;

                        windowContainer.style.width = newWidth + "px";
                        windowContainer.style.height = newHeight + "px";
                    }
                    break;
            }

            document.onmouseup = () => {
                document.onmousemove = null;

                document.onmouseup = e => {
                    const li = document.querySelectorAll(".menu-option");
                    const dropdown = document.querySelectorAll(".dropdown");

                    li.forEach((el, i) => {
                        if (!el.contains(e.target)) {
                            dropdown[i].style.display = "none";
                            el.classList.remove("hover", "active");

                            document.querySelectorAll(".menu-option").forEach(el => el.classList.remove("clicked"));
                        }
                    });
                }
            }
        },
        progressBarAnimation() {
            let i = -75;

            const intervalId = setInterval(() => {
                if (i > 75) {
                    clearInterval(intervalId);
                    return;
                }

                for (const element of this.progressBars) {
                    element.style.background = `linear-gradient(90deg, #06B025 ${25 + i}%, #6CDE72 ${50 + i}%, #06B025 ${75 + i}%)`;
                }

                i += 5;
            }, 50);
        },
        minimizeWindow() {
            this.$emit("on-window-minimized", this.$refs.windowContainer);
        }
    }
};

</script>

<template>

<div
    class="window-container default-pos"
    ref="windowContainer"
    v-if="!closed"
    @click='$emit("on-window-focus")'
>
    <div class="border-top" v-if="!bordersHidden" @mousedown='(event) => resize(event, "up")'></div>
    <div class="border-bottom" v-if="!bordersHidden" @mousedown='(event) => resize(event, "down")'></div>
    <div class="border-left" v-if="!bordersHidden" @mousedown='(event) => resize(event, "left")'></div>
    <div class="border-right" v-if="!bordersHidden" @mousedown='(event) => resize(event, "right")'></div>
    <div class="border-top-left" v-if="!bordersHidden" @mousedown='(event) => resize(event, "top-left")'></div>
    <div class="border-top-right" v-if="!bordersHidden" @mousedown='(event) => resize(event, "top-right")'></div>
    <div class="border-bottom-left" v-if="!bordersHidden" @mousedown='(event) => resize(event, "bottom-left")'></div>
    <div class="border-bottom-right" v-if="!bordersHidden" @mousedown='(event) => resize(event, "bottom-right")'></div>

    <div class="window" ref="window">
        <div class="window-titlebar" @mousedown="moveWindow">
            <span class="window-title">{{ title }}</span>
            <div class="window-title-buttons">
                <button class="titlebar-button minimize" @click="minimizeWindow"></button>
                <button class="titlebar-button maximize" ref="maximize" @click="maximizeWindow"></button>
                <button class="titlebar-button close" @click="closeWindow"></button>
            </div>
        </div>
        <div class="window-content">
            <div class="controls">
                <slot />
            </div>

            <div class="status-bar" v-if="statusBarText || statusBar">
                <span v-if="statusBarText">{{ statusBarText }}</span>
                <span v-if="!statusBarText" style="margin: 9pt"></span>

                <img src="@/assets/icons/right_corner.png" class="right-corner" />
            </div>
        </div>
    </div>
</div>

</template>
