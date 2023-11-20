<script>

import "@/styles/windows10js.scss";
import TesteApp from "./apps/TesteApp.vue";
import { Taskbar, TaskbarItem, Desktop, Windows, StartMenu, StartMenuApp } from "./components/modules";

export default {
    setup() {
        useHead({
            title: "Windows.js"
        });
    },
    mounted() {
        document.addEventListener("mouseup", event => {
            if (!event.target.classList.contains("start-button") && this.startMenuOpened) {
                this.startMenuOpened = false;

                this.startButtonClass = "";
                this.startMenuStyle = "display: block; animation: close-start-menu .1s";
                this.appsStyle = "animation: close-apps .1s";

                setTimeout(() => {
                    this.startMenuStyle = "display: none";
                }, 50);
            }
        });
    },
    components: {
        TesteApp,
        Taskbar,
        Desktop,
        Windows,
        TaskbarItem,
        StartMenu,
        StartMenuApp
    },
    data() {
        return {
            openedApps: [],
            openedAppNames: [],
            appsStyle: "",
            startMenuStyle: "",
            isAppFocused: {
                TesteApp: false
            },
            minimizedApps: []
        };
    },
    methods: {
        openStartMenu() {
            if (!this.startMenuOpened) {
                this.startMenuOpened = true;

                this.startButtonClass = "start-menu-opened";
                this.startMenuStyle = "display: block; animation: open-start-menu .3s";
                this.appsStyle = "animation: open-apps .3s";
            } else {
                this.startMenuOpened = false;

                this.startButtonClass = "";
                this.startMenuStyle = "display: block; animation: close-start-menu .1s";
                this.appsStyle = "animation: close-apps .1s";

                setTimeout(() => {
                    this.startMenuStyle = "display: none";
                }, 50);
            }
        },
        openApp(app, target) {
            if (!this.isAppOpened(app)) {
                switch (app) {
                    case "TesteApp":
                        this.openedApps.push(TesteApp);
                        this.openedAppNames.push("TesteApp");
                        this.isAppFocused.TesteApp = true;
                        break;
                }
            } else {
                if (this.openedAppNames.includes(app)) {
                    const taskbarRect = this.$el.querySelector(`.taskbar-item#${target}`)
                        .getBoundingClientRect();
                    const appRect = this.$el.querySelector(`.window-container#${app}`).getBoundingClientRect();

                    const dX = (taskbarRect.left - appRect.left) - appRect.width / 2;
                    const dY = taskbarRect.top - appRect.top;

                    target.style.transform = `translate(${dX}px, ${dY}px) scale(0)`;

                    setTimeout(() => {
                        target.style.display = 'none';
                    }, 500);

                    this.minimizedApps.push(target);
                }
            }
        },
        isAppOpened(app) {
            return this.openedAppNames.includes(app);
        },
        isAppMinimized(app) {
            return this.minimizedApps.includes(app);
        },
        closeApp(app) {
            this.openedApps.splice(this.openedApps.indexOf(app), 1);
            this.openedAppNames.splice(this.openedAppNames.indexOf(app.name), 1);

            for (let apps in this.isAppFocused) {
                this.isAppFocused[apps] = false;
            }
        },
        focusApp(app) {
            for (let apps in this.isAppFocused) {
                this.isAppFocused[apps] = false;
            }

            this.isAppFocused[app.name] = true;
        },
        minimizeApp(target) {
            const taskbarRect = this.$el.querySelector(`.taskbar-item#${target.id}`)
                .getBoundingClientRect();
            const appRect = target.getBoundingClientRect();

            const dX = (taskbarRect.left - appRect.left) - appRect.width / 2;
            const dY = taskbarRect.top - appRect.top;

            target.style.transform = `translate(${dX}px, ${dY}px) scale(0)`;

            setTimeout(() => {
                target.style.display = 'none';
            }, 500);

            this.minimizedApps.push(target.id);
        }
    }
};

</script>

<template>

<windows>
    <desktop
        :opened-apps="openedApps"
        @on-app-closed="closeApp"
        @on-app-focus="focusApp"
        @on-app-minimized="minimizeApp"
    />

    <taskbar @start-button-clicked="openStartMenu">
        <taskbar-item label="Bloco de Notas" icon="bloco_de_notas" />
        <taskbar-item label="Calculadora"    icon="calculadora" />
        <taskbar-item
            label="Teste"
            icon="unknown_app"
            @click='target => openApp("TesteApp", target)'
            :opened='isAppOpened("TesteApp")'
            :shown="isAppFocused.TesteApp"
            target="TesteApp"
        />
    </taskbar>

    <start-menu :start-menu-style="startMenuStyle" :apps-style="appsStyle">
        <start-menu-app label="Bloco de Notas" icon="bloco_de_notas" with-app-group="B" />
        <start-menu-app label="Calculadora" icon="calculadora" with-app-group="C" />
        <start-menu-app
            label="TesteApp"
            icon="unknown_app"
            with-app-group="T"
            @click='openApp("TesteApp")'
        />
    </start-menu>
</windows>

</template>
