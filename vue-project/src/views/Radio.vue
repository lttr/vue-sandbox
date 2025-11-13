<template>
  <div>
    <div>
      <b>Native radio input has attributes: id, value, name, type="radio", checked </b>
    </div>

    <section>
      <div>With v-model with initial value</div>
      <div>Picked: {{ picked }}</div>

      <input type="radio" id="one" value="One" v-model="picked" />
      <label for="one">One</label>

      <br />

      <input type="radio" id="two" value="Two" v-model="picked" />
      <label for="two">Two</label>
    </section>

    <section>
      <div>With v-model without initial value, but with checked attribute</div>
      <div>Picked: {{ picked2 }}</div>

      <input type="radio" id="three" value="Three" v-model="picked2" />
      <label for="three">Not Checked</label>

      <br />

      <input type="radio" id="four" value="Four" v-model="picked2" checked />
      <label for="four">Checked</label>
    </section>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const picked = ref('One')
const picked2 = ref()
</script>

<style scoped>
section {
  border: 1px dotted grey;
  padding: 1rem;
  margin-block: 1rem;
}

b {
  font-weight: bold;
}
</style>
