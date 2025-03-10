<script setup lang="ts" generic="T">
import { computed, onMounted, type PropType } from "vue";

import { getOption } from "@/utils/config";
import { uuid } from "@/utils/helpers";
import { defineClasses } from "@/composables";

import { injectDropdown } from "./useDropdownShare";

import type { ComponentClass, DynamicComponent } from "@/types";

/**
 * @displayName Dropdown Item
 */
defineOptions({
    isOruga: true,
    name: "ODropdownItem",
    configField: "dropdown",
});

const props = defineProps({
    /** The value that will be returned on events and v-model */
    value: {
        type: [String, Number, Boolean, Object, Array] as PropType<T>,
        default: () => uuid(),
    },
    /** Item label, unnecessary when default slot is used */
    label: { type: String, default: undefined },
    /** Item is disabled */
    disabled: { type: Boolean, default: false },
    /** Item is clickable and emit an event */
    clickable: { type: Boolean, default: true },
    /** Dropdown item tag name */
    tag: {
        type: [String, Object, Function] as PropType<DynamicComponent>,
        default: () => getOption<DynamicComponent>("dropdown.itemTag", "div"),
    },
    /** Set the tabindex attribute on the dropdown item div (-1 to prevent selection via tab key) */
    tabindex: { type: [Number, String], default: 0 },
    /**
     * Role attribute to be passed to the list item for better accessibility.
     * Use menuitem only in situations where your dropdown is related to a navigation menu.
     * @values listitem, menuitem, button
     */
    ariaRole: {
        type: String,
        default: getOption("dropdown.itemAriaRole", "listitem"),
    },
    // class props (will not be displayed in the docs)
    /** Class of the dropdown item */
    itemClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the dropdown item when active  */
    itemActiveClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the dropdown item when clickable */
    itemClickableClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the dropdown item when disabled */
    itemDisabledClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
});

const emits = defineEmits<{
    /**
     * onclick event
     * @param value {[String, Number, Boolean, Object, Array]} value prop data
     * @param event {event} Native Event
     */
    (e: "click", value: T, event: Event): void;
}>();

// inject parent dropdown component if used inside one
const { parentDropdown } = injectDropdown<T>();

onMounted(() => {
    if (!parentDropdown.value)
        throw new Error("You should wrap oDropdownItem on a oDropdown");
});

const isClickable = computed(
    () =>
        !parentDropdown.value.props.disabled &&
        !props.disabled &&
        props.clickable,
);

const isActive = computed(() => {
    if (parentDropdown.value.selected === null) return false;
    if (parentDropdown.value.props.multiple)
        return parentDropdown.value.selected.indexOf(props.value as T) >= 0;
    return props.value === parentDropdown.value.selected;
});

/** Click listener, select the item. */
function selectItem(event: Event): void {
    if (!isClickable.value) return;
    parentDropdown.value.selectItem(props.value as T);
    emits("click", props.value as T, event);
}

// --- Computed Component Classes ---

const rootClasses = defineClasses(
    ["itemClass", "o-drop__item"],
    [
        "itemDisabledClass",
        "o-drop__item--disabled",
        null,
        computed(() => parentDropdown.value.props.disabled || props.disabled),
    ],
    ["itemActiveClass", "o-drop__item--active", null, isActive],
    ["itemClickableClass", "o-drop__item--clickable", null, isClickable],
);
</script>

<template>
    <component
        :is="tag"
        :class="rootClasses"
        :role="ariaRole"
        :tabindex="tabindex"
        data-oruga="dropdown-item"
        @click="selectItem">
        <!--
            @slot Override the label, default is label prop 
        -->
        <slot>{{ label }}</slot>
    </component>
</template>
