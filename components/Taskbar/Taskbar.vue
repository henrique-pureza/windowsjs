<script>

export default {
    name: "Taskbar",
    mounted() {
        this.clock();

        setInterval(() => {
            this.clock();
        }, 1000);
    },
    data() {
        return {
            datetime: "",
            startMenuOpened: false,
            startButtonClass: "",
        }
    },
    methods: {
        clock() {
            const date    = new Date();
            let   hours   =  date.getHours   ();
            let   minutes =  date.getMinutes ();
            let   day     =  date.getDate    ();
            let   month   = (date.getMonth   () + 1);
            let   year    =  date.getFullYear();

            day     = parseInt(day)     < 10 ? "0" + day     : day;
            month   = parseInt(month)   < 10 ? "0" + month   : month;
            year    = parseInt(year)    < 10 ? "0" + year    : year;
            hours   = parseInt(hours)   < 10 ? "0" + hours   : hours;
            minutes = parseInt(minutes) < 10 ? "0" + minutes : minutes;

            this.datetime = `${hours}:${minutes}
${day}/${month}/${year}`;
        },
        startButtonEnter(event) {
            event.target.style.transition = "";
        },
        startButtonLeave(event) {
            event.target.style.transition = "background-color .1s";
        },
        notifyItemLeave(event) {
            event.target.style.transition = "background-color .1s";

            setTimeout(() => {
                event.target.style.transition = "";
            }, 100);
        },
        openStartMenu() {
            this.$emit("start-button-clicked");
        }
    }
};

</script>

<template>

<div class="taskbar">
    <div
        class="start-button"
        :class="startButtonClass"
        title="Iniciar"
        @click="openStartMenu"
        @mouseenter="startButtonEnter"
        @mouseleave="startButtonLeave"
    ></div>
    <div class="taskbar-items">
        <slot />
    </div>
    <div class="notification-area">
        <div class="notify-item" title="Mostrar ícones ocultos" @mouseleave="notifyItemLeave">
            <img src="@/assets/icons/hidden_notify_area_icons.png" />
        </div>
        <div class="notify-item" title="Totalmente carregada (100%)" @mouseleave="notifyItemLeave">
            <img src="@/assets/icons/battery.png" />
        </div>
        <div class="notify-item" title="Acesso à internet" @mouseleave="notifyItemLeave">
            <img src="@/assets/icons/wireless.png" />
        </div>
        <div class="notify-item" title="Fones de ouvido/Alto-falantes: 100%" @mouseleave="notifyItemLeave">
            <img src="@/assets/icons/sound.png" />
        </div>
        <div class="notify-item datetime" @mouseleave="notifyItemLeave">{{ datetime }}</div>
        <div class="notify-item action-center" title="Não há notificações novas" @mouseleave="notifyItemLeave">
            <img src="@/assets/icons/action_center.png" />
        </div>
        <div class="show-desktop" @mouseleave="notifyItemLeave"></div>
    </div>
</div>

</template>
