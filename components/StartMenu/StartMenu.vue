<script lang="ts">

export default {
    name: "StartMenu",
    props: {
        startMenuStyle: String,
        appsStyle: String,
    }
}

</script>

<template>

<div class="start-menu" :style="startMenuStyle">
    <div class="left-panel">
        <div class="left-panel-item expand" title="Iniciar (botão de menu apenas decorativo)"></div>
        <div class="left-panel-item-group">
            <div class="left-panel-item account" title="Henrique Pureza Queiroz"></div>
            <div class="left-panel-item documents" title="Documentos"></div>
            <div class="left-panel-item pictures" title="Imagens"></div>
            <div class="left-panel-item config" title="Configurações"></div>
            <div class="left-panel-item power" title="Desligar"></div>
        </div>
    </div>
    <div class="apps" :style="appsStyle">
        <slot />
    </div>
</div>

</template>
