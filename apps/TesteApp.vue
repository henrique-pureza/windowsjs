<script lang="ts">

import {
    Window,
    MenuBar,
    MenuOption,
    Dropdown,
    DropdownOption,
    GroupBox,
    WinButton,
    CheckBox,
    TextBox,
    ComboBox,
    ComboBoxOption,
    WinLink,
    InputNumber,
    RadioButton,
    ProgressBar,
    InputRange
} from "@/components/modules";

export default {
    name: "TesteApp",
    components: {
        Window,
        MenuBar,
        MenuOption,
        Dropdown,
        DropdownOption,
        GroupBox,
        WinButton,
        CheckBox,
        TextBox,
        ComboBox,
        ComboBoxOption,
        WinLink,
        InputNumber,
        RadioButton,
        ProgressBar,
        InputRange
    }
};

</script>

<template>

<window
    title="Teste"
    status-bar-text="Teste"
    @on-window-closed='$emit("on-app-closed")'
    @on-window-focus='$emit("on-app-focus")'
    @on-window-minimized='(event) => $emit("on-app-minimized", event)'
>
    <menu-bar>
        <menu-option>
            Arquivo
            <dropdown>
                <dropdown-option> Bom dia família amada! </dropdown-option>
                <dropdown-option> Bom dia família amada! </dropdown-option>
            </dropdown>
        </menu-option>
        <menu-option>
            Editar
            <dropdown>
                <dropdown-option>Sair</dropdown-option>
            </dropdown>
        </menu-option>
        <menu-option>
            Exibir
            <dropdown>
                <dropdown-option>Sair</dropdown-option>
            </dropdown>
        </menu-option>
        <menu-option>
            Ajuda
            <dropdown>
                <dropdown-option>Sair</dropdown-option>
            </dropdown>
        </menu-option>
    </menu-bar>

    <group-box
        label="groupBox1"
        style="margin-top: 10px"
    >
        <win-button>Button1</win-button>

        <div style="display: flex; align-items: center">
            <check-box
                id="checkBox1"
                label="CheckBox1"
            />
        </div>

        <text-box />
    </group-box>

    <combo-box>
        <combo-box-option value="">Valor</combo-box-option>
        <combo-box-option value="Teste">Teste</combo-box-option>
        <combo-box-option value="Teste2">Teste2</combo-box-option>
    </combo-box>

    <win-link to="/">Link</win-link>

    <input-number
        min="0"
        max="10"
        step="1"
        value="0"
    />

    <div style="display: flex; align-items: center">
        <radio-button
            id="radioButton1"
            name="radioButton"
            label="radioButton1"
            checked
        />
        <radio-button
            id="radioButton2"
            name="radioButton"
            label="radioButton2"
        />
    </div>

    <progress-bar completed="50" />

    <input-range />
</window>

</template>
