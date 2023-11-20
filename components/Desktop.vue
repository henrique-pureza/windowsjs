<script>

export default {
    name: "Desktop",
    props: ["openedApps"]
};

</script>

<template>
    <div class="desktop">
        <component
            v-for="(appName, index) in openedApps"
            :key="index"
            :is="appName"
            :id="appName.name"
            @on-app-closed='$emit("on-app-closed", appName)'
            @on-app-focus='$emit("on-app-focus", appName)'
            @on-app-minimized='(event) => $emit("on-app-minimized", event)'
        />
    </div>
</template>
