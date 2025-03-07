<script setup lang="ts">
import { ref, computed, toRaw, type PropType, type Ref } from "vue";

import { getOption } from "@/utils/config";
import {
    defineClasses,
    usePropBinding,
    useProviderChild,
    useProviderParent,
    type ProviderItem,
} from "@/composables";

import type { ComponentClass, DynamicComponent } from "@/types";

/**
 * A menu list item
 * @displayName Menu Item
 */
defineOptions({
    isOruga: true,
    name: "OMenuItem",
    configField: "menu",
    inheritAttrs: false,
});

const props = defineProps({
    /** Override existing theme classes completely */
    override: { type: Boolean, default: undefined },
    /** The active state of the menu item, use v-model:active to make it two-way binding. */
    active: { type: Boolean, default: false },
    /** Menu item label */
    label: { type: String, default: undefined },
    /** Menu item will be expanded */
    expanded: { type: Boolean, default: false },
    /** Menu item will be disabled */
    disabled: { type: Boolean, default: false },
    /** Icon to be shown */
    icon: { type: String, default: undefined },
    /**
     * Icon pack to use
     * @values mdi, fa, fas and any other custom icon pack
     */
    iconPack: {
        type: String,
        default: () => getOption("menu.iconPack"),
    },
    /**
     * Icon size
     * @values small, medium, large
     */
    iconSize: {
        type: String,
        default: () => getOption("menu.iconSize"),
    },
    /** Transition name to apply on menu list */
    animation: {
        type: String,
        default: () => getOption("menu.animation", "slide"),
    },
    /** Menu item tag name */
    tag: {
        type: [String, Object, Function] as PropType<DynamicComponent>,
        default: () => getOption<DynamicComponent>("menu.menuTag", "a"),
    },
    /**
     * Role attribute to be passed to the list item for better accessibility.
     * @values listitem, menuitem
     */
    ariaRole: {
        type: String,
        default: getOption("menu.itemAriaRole", "menuitem"),
    },
    // class props (will not be displayed in the docs)
    /** Class of the menu item */
    itemClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the active menu item */
    itemActiveClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the disabled menu item */
    itemDisabledClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the icon of menu item */
    itemIconTextClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the menu item when is a submenu */
    itemSubmenuClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the root element of menu item */
    itemWrapperClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
});

const emits = defineEmits<{
    /**
     * modelValue prop two-way binding
     * @param value {boolean} updated modelValue prop
     */
    (e: "update:modelValue", value: boolean): void;
    /**
     * expanded prop two-way binding
     * @param value {boolean} updated expanded prop
     */
    (e: "update:expanded", value: boolean): void;
}>();

const providedData = computed(() => ({
    reset,
}));

// Inject functionalities and data from the parent menu component
const { parent, item } = useProviderChild<Ref<any>>({
    data: providedData,
});

// Inject functionalities and data from the parent menu-item component
const providedItem = useProviderChild<
    Ref<{ triggerReset: typeof triggerReset } | undefined>
>({
    key: "menu-item",
    needParent: false,
});

const itemParent = computed(() => providedItem.parent?.value);

const isActive = usePropBinding("active", props, emits, { passive: true });

const isExpanded = usePropBinding("expanded", props, emits, { passive: true });

/** template identifier */
const identifier = computed(() =>
    itemParent.value
        ? `menu-item-${providedItem.item.value.identifier}`
        : `menu-${item.value.identifier}`,
);

function onClick(): void {
    if (props.disabled) return;
    triggerReset();
    isExpanded.value = props.expanded || !isExpanded.value;
    if (parent.value.activable) isActive.value = true;
}

function triggerReset(child?: ProviderItem): void {
    // The point of this method is to collect references to the clicked item and any parent,
    // this way we can skip resetting those elements.
    if (itemParent.value?.triggerReset) {
        itemParent.value.triggerReset(toRaw(item.value));
    }
    // else if not a sub item reset parent menu
    else if (parent.value.resetMenu) {
        parent.value.resetMenu([toRaw(item.value), child]);
    }
}

function reset(): void {
    if (parent.value.accordion) isExpanded.value = false;
    if (parent.value.activable) isActive.value = false;
}

const rootRef = ref();

// Provided data is a computed ref to enjure reactivity.
const provideData = computed(() => ({
    triggerReset,
}));

/** Provide functionalities and data to child item components */
useProviderParent(rootRef, { data: provideData, key: "menu-item" });

// --- Computed Component Classes ---

const itemClasses = defineClasses(
    ["itemClass", "o-menu__item"],
    ["itemActiveClass", "o-menu__item--active", null, isActive],
    [
        "itemDisabledClass",
        "o-menu__item--disabled",
        null,
        computed(() => props.disabled),
    ],
    [
        "itemIconTextClass",
        "o-menu__item--icon-text",
        null,
        computed(() => !!props.icon),
    ],
);

const submenuClasses = defineClasses([
    "itemSubmenuClass",
    "o-menu__item__submenu",
]);

const wrapperClasses = defineClasses([
    "itemWrapperClass",
    "o-menu__item__wrapper",
]);
</script>

<template>
    <li
        ref="rootRef"
        :role="ariaRole"
        :class="wrapperClasses"
        :data-id="identifier"
        data-oruga="menu-item"
        aria-roledescription="item">
        <component
            :is="tag"
            v-bind="$attrs"
            :class="itemClasses"
            @click="onClick()">
            <o-icon
                v-if="icon"
                :icon="icon"
                :pack="iconPack"
                :size="iconSize" />
            <!-- 
                @slot Override label
                @binding {boolean} expanded - item expanded state
                @binding {boolean} active - item active state
            -->
            <slot name="label" :expanded="isExpanded" :active="isActive">
                <span>{{ label }}</span>
            </slot>
        </component>

        <!-- sub menu items -->
        <template v-if="$slots.default">
            <transition :name="animation">
                <ul v-show="isExpanded" :class="submenuClasses">
                    <!--
                        @slot Place menu items here 
                    -->
                    <slot />
                </ul>
            </transition>
        </template>
    </li>
</template>
