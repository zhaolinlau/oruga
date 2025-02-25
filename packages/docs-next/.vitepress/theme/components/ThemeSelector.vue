<script setup>
import { ref, watch } from "vue";

// Import theme definitions
import themes from "../../themes.json";

const props = defineProps(["theme"]);
const emits = defineEmits(["update:theme"]);

const selectedTheme = ref();

// implement v-model for theme prop
watch(
    () => props.theme,
    (theme) => (selectedTheme.value = theme),
);
watch(selectedTheme, (theme) => emits("update:theme", theme));

selectedTheme.value = loadTheme();

// load last used theme or set a default one
function loadTheme() {
    const cache = localStorage.getItem("oruga-ui.com:theme");
    if (cache && cache !== "undefined") {
        try {
            const themeConfig = JSON.parse(cache);
            if (themeConfig && typeof themeConfig === "object")
                return themeConfig;
        } catch (e) {
            return themes[0];
        }
    }
    return themes[0];
}

function onThemeChange(theme) {
    selectedTheme.value = theme;
    localStorage.setItem("oruga-ui.com:theme", JSON.stringify(theme));
    location.reload();
}
</script>

<template>
    <o-dropdown
        v-model="selectedTheme"
        root-class="theme-selector"
        aria-role="list"
        @change="onThemeChange">
        <template #trigger="{ active }">
            <span role="button">
                Theme ➜ {{ selectedTheme.label }}
                <o-icon :icon="active ? 'caret-up' : 'caret-down'" />
            </span>
        </template>
        <o-dropdown-item
            v-for="item in themes"
            :key="item.value"
            :value="item"
            clickable
            aria-role="listitem">
            {{ item.label }}
        </o-dropdown-item>
    </o-dropdown>
</template>

<style lang="scss" scoped>
.theme-selector {
    margin-left: 25px;
    line-height: var(--vp-nav-height-mobile);
    font-size: 14px;
    font-weight: 500;

    [role="listitem"],
    [role="button"] {
        cursor: pointer;
    }
}

// default override for oruga base theme
:deep(.o-drop__menu) {
    position: absolute;
    left: 0;
    top: 100%;
    display: block;
    min-width: var(--oruga-dropdown-menu-width, 12rem);
    z-index: var(--oruga-dropdown-menu-zindex, 20);
    background-color: var(--oruga-dropdown-menu-background, #ffffff);
    border-radius: var(--oruga-dropdown-menu-border-radius, 4px);
    box-shadow: var(
        --oruga-dropdown-menu-box-shadow,
        0 0.5em 1em -0.125em rgba(10, 10, 10, 0.1),
        0 0 0 1px rgba(10, 10, 10, 0.02)
    );
    padding: var(--oruga-dropdown-menu-padding, 0.5rem 0 0.5rem 0);
    margin: var(--oruga-dropdown-menu-margin, 0);
}
:deep(.o-drop__item) {
    display: block;
    position: relative;
    cursor: pointer;
    color: var(--oruga-dropdown-item-color, #000000);
    font-size: var(--oruga-dropdown-item-font-size, 1rem);
    font-weight: var(--oruga-dropdown-item-font-weight, 400);
    line-height: var(--oruga-dropdown-item-line-height, 1.5);
    padding: var(--oruga-dropdown-item-padding, 0.375rem 1rem);
    &:hover {
        background-color: var(
            --oruga-dropdown-item-hover-background-color,
            #f5f5f5
        );
        color: var(--oruga-dropdown-item-hover-color, #000000);
    }
}
</style>
