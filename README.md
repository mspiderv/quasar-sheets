# Quasar 2 - Sheets Component

## Screenshot

<img src="https://raw.githubusercontent.com/mspiderv/quasar-sheets/master/screenshot.png" alt="Quasar Sheets Screenshot" />

## Usage

```vue
<template>
  <sheet title="Sheet A" ref="sheetA">
    <p>This is some sheet A content ...</p>
  </sheet>
  <q-btn label="Show Sheet A" @click="$refs.sheetA.show()" />
  <q-btn label="Hide Sheet A" @click="$refs.sheetA.hide()" />
</template>
```

### Sheet Component Implementation

```vue
<template>
  <q-dialog
    ref="sheet"
    v-model="modelValue"
    v-bind="dialogAttrs"
  >
    <div
      class="bg-white"
      :style="style"
      v-bind="$attrs"
    >
      <slot name="toolbar" :hide="hide" :title="title">
        <q-toolbar class="q-px-none q-pb-lg">
          <slot name="hide-button" :hide="hide">
            <q-btn
              flat
              round
              icon="close"
              v-if="hideButton"
              @click="hide()"
            />
          </slot>
          <q-toolbar-title v-if="title || $slots.title">
            <slot name="title" :hide="hide" :title="title">{{ title }}</slot>
          </q-toolbar-title>
        </q-toolbar>
      </slot>
      <div :class="contentClass" :style="contentStyle" v-if="$slots.default">
        <slot />
      </div>
    </div>
  </q-dialog>
</template>

<script>
import { ref, watch, computed, reactive } from 'vue'

const sheets = reactive([])

export default {
  name: 'SheetComponent',
  inheritAttrs: false,
  emits: [
    'update:modelValue',
    'show',
    'hide',
  ],
  props: {
    title: {
      type: String,
    },
    contentClass: {},
    contentStyle: {},
    dialogAttrs: {
      type: Object,
      default () {
        return {
          maximized: true,
          position: 'right',
        }
      },
    },
    modelValue: {
      type: Boolean,
      default: false,
    },
    hideButton: {
      type: Boolean,
      default: true,
    }
  },
  setup (props, { emit }) {
    const sheet = ref(null)
    const modelValue = ref(props.modelValue)
    const order = computed(() => sheets.length - sheets.indexOf(sheet) - 1)

    function show () {
      modelValue.value = true
    }

    function hide () {
      modelValue.value = false
    }

    watch(
      () => props.modelValue,
      (newModelValue) => {
        emit('update:modelValue', modelValue.value = newModelValue)
      }
    )

    watch(
      modelValue,
      (newModelValue) => {
        if (newModelValue) {
          sheets.push(sheet)
          emit('show')
        } else {
          const index = sheets.indexOf(sheet);
          if (index !== -1) {
            sheets.splice(index, 1)
          }
          emit('hide')
        }
        emit('update:modelValue', newModelValue)
      }
    )

    const style = computed(() => {
      return {
        transition: `width ease-out 300ms`,
        width: `${80 + order.value * 3}vw`
      }
    })

    return {
      show,
      hide,
      style,
      modelValue,
      showing: modelValue,
    }
  }
}
</script>
```
