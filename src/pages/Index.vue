<template>
  <q-page padding class="q-gutter-x-md">

    <h5>Sheets</h5>

    <sheet title="Sheet A" class="q-pa-md" ref="sheetA">
      <p>This is some sheet A content ...</p>
      <div class="q-gutter-x-md">
        <q-btn label="Show Sheet B" color="green" @click="$refs.sheetB.show()" />
        <q-btn label="Show Sheet C" color="red" @click="$refs.sheetC.show()" />
      </div>
    </sheet>
    <q-btn label="Show Sheet A" color="blue" @click="$refs.sheetA.show()" />

    <sheet title="Sheet B" class="q-pa-md" ref="sheetB">
      <p>This is some sheet B content ...</p>
      <div class="q-gutter-x-md">
        <q-btn label="Show Sheet A" color="blue" @click="$refs.sheetA.show()" />
        <q-btn label="Show Sheet C" color="red" @click="$refs.sheetC.show()" />
      </div>
    </sheet>
    <q-btn label="Show Sheet B" color="green" @click="$refs.sheetB.show()" />

    <sheet title="Sheet C" class="q-pa-md" ref="sheetC">
      <p>This is some sheet C content ...</p>
      <div class="q-gutter-x-md">
        <q-btn label="Show Sheet A" color="blue" @click="$refs.sheetA.show()" />
        <q-btn label="Show Sheet B" color="green" @click="$refs.sheetB.show()" />
      </div>
    </sheet>
    <q-btn label="Show Sheet C" color="red" @click="$refs.sheetC.show()" />

  </q-page>
</template>

<script>
import Sheet from 'components/Sheet'
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'PageIndex',
  components: {
    Sheet,
  },
  setup () {
    const sheet2 = ref(false)
    const sheet3 = ref(false)

    return {
      sheet2,
      sheet3,
    }
  }
})
</script>
