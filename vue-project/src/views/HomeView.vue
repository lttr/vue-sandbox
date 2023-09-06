<template>
  <div>
    <h2>Reactivity</h2>

    <section>
        I have a <b>ref</b> <code>foo</code> with value <code>{{ foo }}</code>
        <button @click="foo++">Increase foo</button>
    </section>

    <section>
      I have a <b>ref</b> <code>bar</code> with an object
      <pre><code>{{ JSON.stringify(bar) }}</code></pre>
      with source <pre><code>{{ JSON.stringify(sourceForBar) }}</code></pre>
      <button @click="bar.a++">Change bar's a</button>
      <button @click="bar.c.d++">Change bar's d</button>
      <button @click="sourceForBar.b++">Change bar's source b</button>
      <button @click="bar.c = { d: 12 }">Replace bar's c</button>
      <button @click="bar = {a: 20, b: 40, c: { d: 3 }}">Replace bar completely</button>
      <div><code>bar.a</code> is {{ bar.a }}</div>
      <div><code>a</code> destructured from bar is still {{ aFromBar }}</div>
      <div><code>c</code> destructred from bar is {{ destructredCFromBar }}</div>
      <div><code>toRef(bar.a)</code> is still {{ aRefWrongFromBar }}</div>
      <div><code>toRef(bar, 'a')</code> is {{ aRefFromBar }}</div>
    </section>

    <section>
      I have a <b>reactive</b> <code>baz</code> with an object
      <pre><code>{{ JSON.stringify(baz) }}</code></pre>
      with source <pre><code>{{ JSON.stringify(sourceForBaz) }}</code></pre>
      <button @click="baz.a++">Change baz's a</button>
      <button @click="baz.c.d++">Change baz's d</button>
      <button @click="sourceForBaz.b++">Change baz's source b</button>
      <button @click="baz.c = { d: 12 }">Replace baz's c</button>
      <button @click="baz = {a: 20, b: 40, c: { d: 3 }}">Replace baz completely</button>
      <div><code>baz.a</code> is {{ baz.a }}</div>
      <div><code>a</code> destructured from baz is still {{ aFromBaz }}</div>
      <div><code>c</code> destructred from baz is {{ destructredCFromBaz }}</div>
      <div><code>toRef(baz.a)</code> is still {{ aRefWrongFromBaz }}</div>
      <div><code>toRef(baz, 'a')</code> is {{ aRefFromBaz }}</div>
    </section>

  </div>
</template>

<script setup lang="ts">
import { ref, reactive, toRef } from 'vue';

const foo = ref(0)

const sourceForBar = {
  a: 1,
  b: 2,
  c: { d: 3 }
}
const bar = ref(sourceForBar)
const aFromBar = bar.value.a
const aRefWrongFromBar = toRef(bar.value.a)
const aRefFromBar = toRef(bar.value, 'a')
const { c: destructredCFromBar } = bar.value

const sourceForBaz = {
  a: 1,
  b: 2,
  c: { d: 3 }
}
const baz = reactive(sourceForBaz)
const aFromBaz = baz.a
const aRefWrongFromBaz = toRef(baz.a)
const aRefFromBaz = toRef(baz, 'a')
const { c: destructredCFromBaz } = baz


</script>

<style scoped>
section {
  margin-top: 1rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid grey;
}

button {
  margin: 1ch;
}

pre {
  margin-block: 0.3rem;
}
</style>
