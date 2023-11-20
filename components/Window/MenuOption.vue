<script>

export default {
    name: "MenuOption",
    data() {
        return {
            tempLi: null
        };
    },
    methods: {
        mouseEnter(event) {
            event.target.classList.add("hover");
            this.tempLi = event.target;

            if (event.target.classList.contains("clicked")) {
                event.target.classList.remove("hover");
                event.target.classList.add("active");
                event.target.querySelector(".dropdown").style.display = "block";
            }
        },
        mouseLeave(event) {
            event.target.classList.remove("hover");

            if (event.target.classList.contains("clicked")) {
                event.target.classList.remove("hover");
                event.target.classList.remove("active");
                this.tempLi.querySelector(".dropdown").style.display = "none";
            }
        },
        click(event) {
            const li = event.target;

            if (!li.classList.contains("clicked")) {
                if (!li.classList.contains("active")) {
                    li.classList.remove("hover");
                    li.classList.add("active");
                    li.querySelector(".dropdown").style.display = "block";

                    document.querySelectorAll(".menu-option").forEach(el => el.classList.add("clicked"));
                } else {
                    li.classList.remove("active");
                    li.classList.add("hover");
                    li.querySelector(".dropdown").style.display = "none";

                    document.querySelectorAll(".menu-option").forEach(el => el.classList.remove("clicked"));
                }
            } else {
                li.classList.remove("active");
                li.classList.add("hover");
                li.querySelector(".dropdown").style.display = "none";

                document.querySelectorAll(".menu-option").forEach(el => el.classList.remove("clicked"));
            }
        }
    },
    mounted() {
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
}

</script>

<template>
    <li class="menu-option" @mouseenter="mouseEnter" @mouseleave="mouseLeave" @click="click">
        <slot />
    </li>
</template>
