<script>

import _ from 'underscore';
import allData from '../assets/towns.json'

const NO_OF_ROWS = 10

export default {

    data() {
        const BORDER_NOT_SELECT = "1px solid black"
        const BORDER_SELECT = "2px solid black"
        const COLOR_NOT_SELECT = "#eeeeee"

        var selected = undefined // Current selected on the left
        var selection = []
        var reverseSelection = []
        var colorLeft = []
        var colorRight = []
        var borderLeft = []
        var solveMsg = ""
        var quizzData = { "correct": [], "keys": [], "shuffled": []}
        var pallette = []

        var allSelected = false
        var solved = false
        var buttonText = "Comprobar"


        return {
            quizzData: quizzData, 
            pallette: pallette,
            selection: selection,
            reverseSelection: reverseSelection,
            colorLeft: colorLeft,
            colorRight: colorRight,
            borderLeft: borderLeft,
            BORDER_SELECT: BORDER_SELECT,
            BORDER_NOT_SELECT: BORDER_NOT_SELECT,
            COLOR_NOT_SELECT: COLOR_NOT_SELECT,
            allSelected: allSelected,
            solveMsg: solveMsg,
            solved: solved,
            buttonText: buttonText,
        }
    },
    methods: {
        myInit () {
            const PALETTE = ["#9e0142", "#d53e4f", "#f46d43", "#fdae61", "#fee08b", "#e6f598", "#abdda4",
                "#66c2a5", "#3288bd", "#5e4fa2"]
            var keys = Object.keys (allData)

            this.selected = undefined; // Current selected on the left
            this.selection = []
            this.reverseSelection = []
            this.colorLeft = []
            this.colorRight = []
            this.borderLeft = []
            this.solveMsg = ""
            this.quizzData = { "correct": [], "keys": [], "shuffled": []}
            _.sample(keys, NO_OF_ROWS).forEach ((key, i) => {
                var resp = _.sample (allData[key])
                this.quizzData.keys.push (key)
                this.quizzData.correct.push (resp)
                this.selection.push (undefined)
                this.reverseSelection.push (undefined)
                this.colorLeft.push (this.COLOR_NOT_SELECT)
                this.colorRight.push (this.COLOR_NOT_SELECT)
                this.borderLeft.push (this.BORDER_NOT_SELECT)

            })

            this.quizzData.shuffled = _.shuffle (this.quizzData.correct)
            //this.pallette = _.shuffle (PALETTE)
            this.pallette = PALETTE

            this.allSelected = false
            this.solved = false
            this.buttonText = "Comprobar"
        },
        onClickLeft (index) {

            if (this.selected == index) {
                this.selected = undefined
                this.colorLeft[index] = this.COLOR_NOT_SELECT
                this.borderLeft[index] = this.BORDER_NOT_SELECT
            } else {
                this.colorLeft[this.selected] = this.COLOR_NOT_SELECT
                this.borderLeft[this.selected] = this.BORDER_NOT_SELECT
                
                this.colorLeft[index] = this.pallette[index]
                this.borderLeft[index] = this.BORDER_SELECT
                this.selected = index

                if (this.selection[index] !== undefined) {
                    var reverse = this.selection [index]
                    this.colorRight[reverse] = this.COLOR_NOT_SELECT
                    this.selection[index] = undefined
                    this.reverseSelection[reverse] = undefined
                }
            }

        },
        onClickRight (index) {

            if (this.selected !== undefined) {
                if (this.reverseSelection[index] !== undefined) {
                    var direct  = this.reverseSelection [index]
                    this.colorLeft[direct] = this.COLOR_NOT_SELECT
                    this.selection[direct] = undefined
                }
                this.reverseSelection[index] = this.selected
                this.selection[this.selected] = index
                this.colorRight[index] = this.pallette [this.selected]
                this.borderLeft[this.selected] = this.BORDER_NOT_SELECT
                this.selected = undefined
            } else {
                if (this.reverseSelection[index] !== undefined) {
                    var direct  = this.reverseSelection [index]
                    this.colorLeft[direct] = this.COLOR_NOT_SELECT
                    this.selection[direct] = undefined
                    this.reverseSelection[index] = undefined
                    this.colorRight[index] = this.COLOR_NOT_SELECT
                    this.colorLeft[index] = this.COLOR_NOT_SELECT
                }
            }

            var allSelect = true
            for (var i in this.selection) {
                if (this.selection [i] === undefined) {
                    allSelect  = false
                    break
                }
            }
            this.allSelected = allSelect
        },
        onClickSolve () {
            if (! this.solved) {
                var errMsg = "Errores: "
                    var noOfErrors = 0
                    for (var i in this.selection) {
                        var rightIndex = this.selection[i]
                            var guessName = this.quizzData.shuffled[rightIndex]
                            var correctName = this.quizzData.correct[i]
                            if (guessName != correctName) {
                                noOfErrors += 1
                                    errMsg += this.quizzData.keys[i] + " -> " + correctName + "  "
                            } 
                    }
                if (noOfErrors == 0) {
                    errMsg = "Todas correctas."
                }

                this.solveMsg = errMsg
                this.solved = true
                this.buttonText = "Reiniciar"
            } else {
                this.myInit ()
            }
        }
    },
    mounted() {
        this.myInit()
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
        <td><button class="cell" 
                @click="onClickLeft(index)" 
                :style="{ 'background-color': colorLeft[index], 'border': borderLeft[index]}">
                    {{ key }}
        </button></td>
        <td><button class="cell" 
                @click="onClickRight(index)" 
                :style="{ 'background-color': colorRight[index], 'border': BORDER_NOT_SELECT}">
                    {{ quizzData.shuffled[index] }}
        </button></td>
      </tr>
    </tbody>
  </table>
  <div class="bottom_button">
    <button class="check_button" :disabled="! allSelected" @click="onClickSolve">{{ buttonText }}</button>
  </div>
  <p>{{ solveMsg }}</p>

</template>

<style scoped>
table 
{
    border: 0px;
    table-layout: fixed;
    width: 400px;
    display: block;
    margin-left: auto;
    margin-right: auto;
    vertical-align: top;
}

td {
    border: 0px;
    width: 200px;
    align: center;
}

button.cell {
    width: 100%;
    font-size: 20px;
}

button.down {
    display: block;
    margin-left: auto;
    margin-right: auto;
}

div.bottom_button {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 10px;
}

button.check_button {
    font-size: 15px;
}

</style>
