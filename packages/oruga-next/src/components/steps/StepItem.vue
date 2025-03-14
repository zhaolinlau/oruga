<script setup lang="ts">
import { computed, ref, useSlots, type ComputedRef, type PropType } from "vue";

import { getOption } from "@/utils/config";
import { uuid } from "@/utils/helpers";
import { defineClasses, useProviderChild } from "@/composables";

import type { StepsComponent } from "./types";
import type { ComponentClass, DynamicComponent } from "@/types";

/**
 * @displayName Step Item
 */
defineOptions({
    isOruga: true,
    name: "OStepItem",
    configField: "steps",
});

const props = defineProps({
    /** Override existing theme classes completely */
    override: { type: Boolean, default: undefined },
    /** Item value (it will be used as v-model of wrapper component) */
    value: { type: [String, Number], default: () => uuid() },
    /** Item label */
    label: { type: String, default: undefined },
    /** Step marker content (when there is no icon) */
    step: { type: [String, Number], default: undefined },
    /**
     * Default style for the step.
     * This will override parent type.
     * Could be used to set a completed step to "success" for example
     */
    variant: { type: String, default: undefined },
    /**
     * Item can be used directly to navigate.
     * If undefined, previous steps are clickable while the others are not
     */
    clickable: { type: Boolean, default: undefined },
    /** Show/hide item */
    visible: { type: Boolean, default: true },
    /** Icon on the left */
    icon: {
        type: String,
        default: () => getOption("steps.icon"),
    },
    /** Icon pack */
    iconPack: {
        type: String,
        default: () => getOption("steps.iconPack"),
    },
    /** Step item tag name */
    tag: {
        type: [String, Object, Function] as PropType<DynamicComponent>,
        default: () => getOption<DynamicComponent>("steps.itemTag", "button"),
    },
    /** Role attribute to be passed to the div wrapper for better accessibility */
    ariaRole: {
        type: String,
        default: () => getOption("steps.ariaRole", "tab"),
    },
    /** Sets a class to the item header */
    headerClass: { type: String, default: undefined },
    // class props (will not be displayed in the docs)
    /** Class of the content item */
    itemClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the nav item */
    itemHeaderClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the nav item when active */
    itemHeaderActiveClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the nav item behind the active one */
    itemHeaderPreviousClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
    /** Class of the nav item with variant (default value by parent steps component) */
    itemHeaderVariantClass: {
        type: [String, Array, Function] as PropType<ComponentClass>,
        default: undefined,
    },
});

const emits = defineEmits<{
    /** on tab item activate event */
    (e: "activate"): void;
    /** on tab item deactivate event */
    (e: "deactivate"): void;
}>();

const slots = useSlots();

const providedData = computed(() => ({
    ...props,
    $slots: slots,
    isTransitioning: isTransitioning.value,
    activate,
    deactivate,
}));

// Inject functionalities and data from the parent carousel component
const { parent, item } = useProviderChild<ComputedRef<StepsComponent>>({
    data: providedData,
});

const transitionName = ref();

const isActive = computed(() => parent.value.activeId === props.value);

const isTransitioning = ref(false);

const nextAnimation = computed(() => {
    const idx =
        parent.value.vertical && parent.value.animation.length === 4 ? 2 : 0;
    return parent.value.animation[idx];
});

const prevAnimation = computed(() => {
    const idx =
        parent.value.vertical && parent.value.animation.length === 4 ? 3 : 1;
    return parent.value.animation[idx];
});

/** Activate element, alter animation name based on the index. */
function activate(oldIndex: number): void {
    transitionName.value =
        item.value.index < oldIndex ? nextAnimation.value : prevAnimation.value;

    // emit event
    emits("activate");
}

/** Deactivate element, alter animation name based on the index. */
function deactivate(newIndex: number): void {
    transitionName.value =
        newIndex < item.value.index ? nextAnimation.value : prevAnimation.value;

    // emit event
    emits("deactivate");
}

/** Transition after-enter hook */
function afterEnter(): void {
    isTransitioning.value = true;
}

/** Transition before-leave hook */
function beforeLeave(): void {
    isTransitioning.value = true;
}

// --- Computed Component Classes ---

const elementClasses = defineClasses(["itemClass", "o-steps__item"]);
</script>

<template>
    <Transition
        :disabled="!parent.animated"
        :name="transitionName"
        :appear="parent.animateInitially"
        @after-enter="afterEnter"
        @before-leave="beforeLeave">
        <div
            v-show="isActive && visible"
            ref="rootRef"
            :class="elementClasses"
            :data-id="`steps-${item.identifier}`"
            data-oruga="steps-item"
            :tabindex="isActive ? 0 : -1"
            :role="ariaRole"
            aria-roledescription="item">
            <!-- 
                @slot Step item content
            -->
            <slot />
        </div>
    </Transition>
</template>
