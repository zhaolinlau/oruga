---
title: Autocomplete
---

# Autocomplete

<div class="vp-doc">

> Extended input that provide suggestions while the user types

</div>

<div class="vp-example">

## Examples

<example-autocomplete />

</div>
<div class="vp-example">

## Class props

<inspector-autocomplete-viewer />

</div>

<div class="vp-doc">

## Autocomplete component

> Extended input that provide suggestions while the user types

```html
<o-autocomplete></o-autocomplete>
```

### Props

| Prop name            | Description                                                                                                                                                                      | Type                                             | Values                                            | Default                                                                                                                                                          |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| animation            | Transition name to apply on dropdown list                                                                                                                                        | string                                           | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;animation: "fade"<br>}</code>         |
| autocomplete         | Native options to use in HTML5 validation                                                                                                                                        | string                                           | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;autocomplete: "off"<br>}</code>       |
| checkScroll          | Makes the component check if list reached scroll start or end and emit scroll events.                                                                                            | boolean                                          | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;checkScroll: false<br>}</code>        |
| clearIcon            | Icon name to be added on the clear button                                                                                                                                        | string                                           | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;clearIcon: "close-circle"<br>}</code> |
| clearOnSelect        | Clear input text on select                                                                                                                                                       | boolean                                          | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;clearOnSelect: false<br>}</code>      |
| clearable            | Add a button/icon to clear the inputed text                                                                                                                                      | boolean                                          | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;clearable: false<br>}</code>          |
| confirmKeys          | Array of keys (https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) which will add a tag when typing (default tab and enter)                           | array                                            | -                                                 | <code style='white-space: nowrap; padding: 0;'>["Tab", "Enter"]</code>                                                                                           |
| data                 | Options / suggestions                                                                                                                                                            | array                                            | -                                                 | <code style='white-space: nowrap; padding: 0;'>[]</code>                                                                                                         |
| debounce             | Number of milliseconds to delay before to emit input event                                                                                                                       | number                                           | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;debounce: 400<br>}</code>             |
| disabled             | Same as native input disabled                                                                                                                                                    | boolean                                          | -                                                 | <code style='white-space: nowrap; padding: 0;'>false</code>                                                                                                      |
| expanded             | Makes input full width when inside a grouped or addon field                                                                                                                      | boolean                                          | -                                                 | <code style='white-space: nowrap; padding: 0;'>false</code>                                                                                                      |
| field                | Property of the object (if data is array of objects) to use as display text, and to keep track of selected option                                                                | string                                           | -                                                 |                                                                                                                                                                  |
| formatter            | Function to format an option to a string for display in the input (as alternative to field prop)                                                                                 | (value: string \| number) =&gt; string \| number | -                                                 |                                                                                                                                                                  |
| groupField           | Property of the object (if `data` is array of objects) to use as display text of group                                                                                           | string                                           | -                                                 |                                                                                                                                                                  |
| groupOptions         | Property of the object (if `data` is array of objects) to use as key to get items array of each group                                                                            | string                                           | -                                                 |                                                                                                                                                                  |
| icon                 | Icon to be shown                                                                                                                                                                 | string                                           | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;icon: undefined<br>}</code>           |
| iconClickable        | Makes the icon clickable                                                                                                                                                         | boolean                                          | -                                                 | <code style='white-space: nowrap; padding: 0;'>false</code>                                                                                                      |
| iconPack             | Icon pack to use                                                                                                                                                                 | string                                           | `mdi`, `fa`, `fas and any other custom icon pack` | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;iconPack: undefined<br>}</code>       |
| iconRight            | Icon to be added on the right side                                                                                                                                               | string                                           | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;iconRight: undefined<br>}</code>      |
| iconRightClickable   | Make the icon right clickable                                                                                                                                                    | boolean                                          | -                                                 | <code style='white-space: nowrap; padding: 0;'>false</code>                                                                                                      |
| iconRightVariant     | Variant of right icon                                                                                                                                                            | string                                           | -                                                 |                                                                                                                                                                  |
| itemTag              | Menu item tag name                                                                                                                                                               | DynamicComponent                                 | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;itemTag: "div"<br>}</code>            |
| keepFirst            | The first option will always be pre-selected (easier to just hit enter or tab)                                                                                                   | boolean                                          | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;keepFirst: false<br>}</code>          |
| keepOpen             | Keep open dropdown list after select                                                                                                                                             | boolean                                          | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;keepOpen: false<br>}</code>           |
| maxHeight            | Max height of dropdown content                                                                                                                                                   | string\|number                                   | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;maxHeight: undefined<br>}</code>      |
| maxlength            | Same as native maxlength, plus character counter                                                                                                                                 | number                                           | -                                                 |                                                                                                                                                                  |
| menuTag              | Menu tag name                                                                                                                                                                    | DynamicComponent                                 | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;menuTag: "div"<br>}</code>            |
| mobileModal          | Dropdown content (items) are shown into a modal on mobile                                                                                                                        | boolean                                          | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;mobileModal: false<br>}</code>        |
| openOnFocus          | Open dropdown list on focus                                                                                                                                                      | boolean                                          | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;openOnFocus: false<br>}</code>        |
| override             | Override existing theme classes completely                                                                                                                                       | boolean                                          | -                                                 |                                                                                                                                                                  |
| placeholder          | Input placeholder                                                                                                                                                                | string                                           | -                                                 |                                                                                                                                                                  |
| position             | Position of the dropdown                                                                                                                                                         | string                                           | `auto`, `top`, `bottom`                           | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;position: "auto"<br>}</code>          |
| rounded              | Makes the element rounded                                                                                                                                                        | boolean                                          | -                                                 | <code style='white-space: nowrap; padding: 0;'>false</code>                                                                                                      |
| selectOnClickOutside | Trigger the select event for the first pre-selected option when clicking outside and `keep-first` is enabled                                                                     | boolean                                          | -                                                 | <code style='white-space: nowrap; padding: 0;'>false</code>                                                                                                      |
| selectableFooter     | Allows the footer in the autocomplete to be selectable                                                                                                                           | boolean                                          | -                                                 | <code style='white-space: nowrap; padding: 0;'>false</code>                                                                                                      |
| selectableHeader     | Allows the header in the autocomplete to be selectable                                                                                                                           | boolean                                          | -                                                 | <code style='white-space: nowrap; padding: 0;'>false</code>                                                                                                      |
| size                 | Size of the control                                                                                                                                                              | string                                           | `small`, `medium`, `large`                        | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;size: undefined<br>}</code>           |
| statusIcon           | Show status icon using field and variant prop                                                                                                                                    | boolean                                          | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>{<br>&nbsp;&nbsp;statusIcon: true<br>}</code>                        |
| teleport             | Append the component to another part of the DOM.<br/>Set `true` to append the component to the body.<br/>In addition, any CSS selector string or an actual DOM node can be used. | boolean\|string\|object                          | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>autocomplete: {<br>&nbsp;&nbsp;teleport: false<br>}</code>           |
| type                 | Input type                                                                                                                                                                       | string                                           | -                                                 | <code style='white-space: nowrap; padding: 0;'>"text"</code>                                                                                                     |
| useHtml5Validation   | Enable html 5 native validation                                                                                                                                                  | boolean                                          | -                                                 | <div><small>From <b>config</b>:</small></div><code style='white-space: nowrap; padding: 0;'>{<br>&nbsp;&nbsp;useHtml5Validation: true<br>}</code>                |
| v-model              |                                                                                                                                                                                  | string\|number                                   | -                                                 | <code style='white-space: nowrap; padding: 0;'>""</code>                                                                                                         |
| validationMessage    | The message which is shown when a validation error occurs                                                                                                                        | string                                           | -                                                 |                                                                                                                                                                  |

### Events

| Event name        | Properties                                             | Description                                    |
| ----------------- | ------------------------------------------------------ | ---------------------------------------------- |
| icon-click        | **event** `Event` - native event                       | on icon click event                            |
| update:modelValue | **value** `string \| number` - updated modelValue prop | modelValue prop two-way binding                |
| input             | **value** `string \| number` - input value             | on input change event                          |
| select            | **value** `string \| number` - selected value          | selected element changed event                 |
| select-header     | **event** `Event` - native event                       | header is selected                             |
| select-footer     | **event** `Event` - native event                       | footer is selected                             |
| focus             | **event** `Event` - native event                       | on input focus event                           |
| blur              | **event** `Event` - native event                       | on input blur event                            |
| invalid           | **event** `Event` - native event                       | on input invalid event                         |
| icon-right-click  | **event** `Event` - native event                       | on icon right click event                      |
| scroll-start      |                                                        | the list inside the dropdown reached the start |
| scroll-end        |                                                        | the list inside the dropdown reached it's end  |

### Slots

| Name    | Description                    | Bindings                                                                                                         |
| ------- | ------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| header  | Define an additional header    |                                                                                                                  |
| group   | Override the option grpup      | **group** `object` - options group<br/>**index** `number` - option index                                         |
| default | Override the select option     | **option** `object` - option object<br/>**value** `unknown` - option value<br/>**index** `number` - option index |
| empty   | Define content for empty state |                                                                                                                  |
| footer  | Define an additional footer    |                                                                                                                  |

</div>

<div class="vp-doc">

## Sass variables

<div class="theme-orugabase">

> Current theme ➜ _[Oruga Base](https://github.com/oruga-ui/theme-oruga)_

| SASS Variable                             | Default                |
| ----------------------------------------- | ---------------------- |
| $autocomplete-item-color                  | #000000                |
| $autocomplete-item-disabled-opacity       | $base-disabled-opacity |
| $autocomplete-item-font-size              | $base-font-size        |
| $autocomplete-item-hover-background-color | #f5f5f5                |
| $autocomplete-item-hover-color            | #000000                |
| $autocomplete-item-line-height            | $base-line-height      |
| $autocomplete-item-padding                | 0.375rem 1rem          |

See ➜ 📄 [Full scss file](https://github.com/oruga-ui/theme-oruga/tree/main/src/assets/scss/components/_autocomplete.scss)

</div><div class="theme-orugafull">

> Current theme ➜ _[Oruga Full](https://github.com/oruga-ui/theme-oruga)_

| SASS Variable                             | Default                |
| ----------------------------------------- | ---------------------- |
| $autocomplete-item-color                  | #000000                |
| $autocomplete-item-disabled-opacity       | $base-disabled-opacity |
| $autocomplete-item-font-size              | $base-font-size        |
| $autocomplete-item-hover-background-color | #f5f5f5                |
| $autocomplete-item-hover-color            | #000000                |
| $autocomplete-item-line-height            | $base-line-height      |
| $autocomplete-item-padding                | 0.375rem 1rem          |

See ➜ 📄 [Full scss file](https://github.com/oruga-ui/theme-oruga/tree/main/src/assets/scss/components/_autocomplete.scss)

</div><div class="theme-bulma">

> Current theme ➜ _[Bulma](https://github.com/oruga-ui/theme-bulma)_

<p>The theme does not have any custom variables for this component.</p>
</div><div class="theme-bootstrap">

> Current theme ➜ _[Bootstrap](https://github.com/oruga-ui/theme-bootstrap)_

| SASS Variable                 | Default |
| ----------------------------- | ------- |
| $autocomplete-menu-max-height | 200px   |

See ➜ 📄 [Full scss file](https://github.com/oruga-ui/theme-bootstrap/tree/main/src/assets/scss/components/_autocomplete.scss)

</div>

</div>
