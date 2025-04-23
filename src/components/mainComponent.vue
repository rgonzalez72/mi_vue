<script>

import _ from 'underscore';
import allData from '../assets/towns.json'

const NO_OF_ROWS = 10

export default {

    data() {
        const PALETTE = ["#9e0142", "#d53e4f", "#f46d43", "#fdae61", "#fee08b", "#e6f598", "#abdda4",
              "#66c2a5", "#3288bd", "#5e4fa2"]
        var keys = Object.keys (allData)

        var quizzData = { "correct": [], "keys": [], "shuffled": []}
        _.sample(keys, NO_OF_ROWS).forEach ((key, i) => {
            var resp = _.sample (allData[key])
            quizzData.keys.push (key)
            quizzData.correct.push (resp)
        })

        quizzData.shuffled = _.shuffle (quizzData.correct)
        var pallette = _.shuffle (PALETTE)
        return {
            quizzData: quizzData, 
            pallette: pallette
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
        <td><button class="cell" :style="{ 'background-color': pallette[index] }">{{ key }}</button></td>
        <td><button class="cell" style="border: 1px solid black">{{ quizzData.shuffled[index] }}</button></td>
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
    font-size: 30px;
}

button.down {
    display: block;
    margin-left: auto;
    margin-right: auto;
}

</style>
