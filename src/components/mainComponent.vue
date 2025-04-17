<script>

import _ from 'underscore';
import allData from '../assets/towns.json'

const NO_OF_ROWS = 10

export default {

    data() {
        var keys = Object.keys (allData)

        var quizzData = { "correct": [], "keys": [], "shuffled": []}
        _.sample(keys, NO_OF_ROWS).forEach ((key, i) => {
            var resp = _.sample (allData[key])
            quizzData.keys.push (key)
            quizzData.correct.push (resp)
        })

        quizzData.shuffled = _.shuffle (quizzData.correct)
        return {
            quizzData: quizzData
        }
    },
}
</script>

<template>
  <table>
    <colgroup>
      <col style="width:50%">
      <col style="width:50%">
    </colgroup>
    <tbody>
      <tr v-for="(key, index) in  quizzData.keys">
        <td><button class="cell">{{ key }}</button></td>
        <td><button class="cell">{{ quizzData.shuffled[index] }}</button></td>
      </tr>
    </tbody>
  </table>

</template>

<style scoped>
table 
{
    border: 0px;
    table-layout: fixed;
    width: 1200px;
    display: block;
    margin-left: auto;
    margin-right: auto;
    vertical-align: top;
}

td {
    border: 0px;
    width: 600px;
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
