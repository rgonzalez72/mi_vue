<script setup>
import { ref } from 'vue'
import _ from 'underscore';

const NO_OF_ROWS = 5
const count = ref(0)

import allData from '../assets/towns.json'

var keys = Object.keys (allData)

var quizzData = {}
_.sample(keys, NO_OF_ROWS).forEach ((key, i) => {
    var resp = _.sample (allData[key])
    console.log (key + " " + resp)
    quizzData[key] = resp
})


defineExpose({
    quizzData
})
</script>

<template>
  <table>
    <colgroup>
      <col style="width:50%">
      <col style="width:50%">
    </colgroup>
    <tbody>
      <tr><td><button class="cell">Uno</button></td><td><button class="cell">111</button></td></tr>
      <tr><td><button class="cell">Dos</button></td><td><button class="cell">222</button></td></tr>
      <tr><td><button class="cell">Tres</button></td><td><button class="cell">333</button></td></tr>
      <tr><td><button class="cell">Cuatro</button></td><td><button class="cell">444</button></td></tr>
      <tr><td><button class="cell">Cinco</button></td><td><button class="cell">555</button></td></tr>
    </tbody>
  </table>
  <button class="down" @click="count++">You clickedme {{ count }} times.</button>

  <p v-for="(key) in Object.keys(quizzData)">
    {{ key }} : {{ quizzData[key] }}
  </p>

</template>

<style scoped>
table 
{
    border: 0px;
    table-layout: fixed;
    width: 600px;
    display: block;
    margin-left: auto;
    margin-right: auto;
    vertical-align: top;
}

td {
    border: 0px;
    width: 300px;
    align: center;
}

button.cell {
    width: 100%;
    font-size: 40px;
}

button.down {
    display: block;
    margin-left: auto;
    margin-right: auto;
}

</style>
