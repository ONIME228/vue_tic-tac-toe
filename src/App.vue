<script setup>
import {ref} from 'vue';

const cellData = ref({
    0:"",
    1:"",
    2:"",
    3:"",
    4:"",
    5:"",
    6:"",
    7:"",
    8:"",
});

const isX = ref(true);
const gameStatus = ref('');
const isNewGame = ref(false);
const X = "X";
const O = "O"; 

const handleClick = (index,value)=>{
    if (value) return;
    cellData.value[index] = isX.value?X:O;
    isX.value = !isX.value;
    validateGame();
}

const validateGame = () => {
    if (gameStatus.value) return;
    const isFirstRow = !!cellData.value[0] && cellData.value[0] === cellData.value[1] && cellData.value[2] === cellData.value[1]
    const isSecondRow = !!cellData.value[3] &&  cellData.value[4] === cellData.value[5] && cellData.value[3] === cellData.value[5] 
    const isThirdRow = !!cellData.value[6] && cellData.value[6] === cellData.value[8] && cellData.value[7] === cellData.value[8];
    const isFirstColumn = !!cellData.value[0] && cellData.value[0] === cellData.value[6] && cellData.value[3] === cellData.value[6];
    const isSecondColumn = !!cellData.value[1] && cellData.value[4] === cellData.value[7] && cellData.value[1] === cellData.value[7];
    const isThirdColumn = !!cellData.value[2] && cellData.value[5] === cellData.value[8] && cellData.value[2] === cellData.value[8];
    const isFirstCross = !!cellData.value[0] && cellData.value[0] === cellData.value[8] && cellData.value[4] === cellData.value[8];
    const isSecondCross = !!cellData.value[2] && cellData.value[2] === cellData.value[6] && cellData.value[4] === cellData.value[6];

    const winMessage = `Player ${isX.value?O:X} wins!`;
    
    const winConditions = [isFirstRow,isSecondRow,isThirdRow,isFirstColumn,isSecondColumn,isThirdColumn,isFirstCross,isSecondCross];
    
    for (const option of winConditions) {
        if (option){
            gameStatus.value = winMessage;
            isNewGame.value = true;
            return;
        } 
    }

    let isCellDataFull = true;  

    for (const cell in cellData.value){
        if (!cellData.value[cell]) {
            isCellDataFull = false;
            break;
        }    
    }
    if (isCellDataFull) {
        gameStatus.value = 'Draw!';
        isNewGame.value = true;
    }
}

const handleStart = () => {
    for (const cell in cellData.value){
        cellData.value[cell] = "";
    }
    isNewGame.value = false;
    gameStatus.value = '';
}
</script>

<template>
    <article class="wrapper">
        <h1 class="header color-white">Tic-Tac-Toe</h1>
        <section class="playground-wrapper">
            <div v-for="(value,index) in cellData" 
                :data-key="index" 
                :key="index" 
                @click.prevent="handleClick(index,value)" 
                class="playground-cell color-white" 
                :class="value===X?'x-color':'o-color'"
            > 
                <button class="cell-button">
                    {{value}}
                </button>
            </div>
        </section>
        <h5 class="color-white"> {{ gameStatus }} </h5>
        <button v-if="isNewGame" @click.prevent="handleStart" class="color-white ng-cta"> Start a new game </button>
    </article>
</template>

<style>
.wrapper {
    padding-top: 48px;
    display: flex;
    flex-direction: column;
    gap: 16px;
    align-items: center;
}
.playground-wrapper{
    display: grid;
    grid-template-columns: repeat(3, 100px);
    gap:0;
    /* position: absolute; */
}
.playground-cell {
    cursor: pointer;
    border: 1px solid white;
    width: 100px;
    height: 100px;
    line-height: 100px;
    font-size: 80px;
    text-align: center;
    /* position: relative; */
    /* z-index: 10; */
}
.color-white {
    color: white;
}
.ng-cta {
    border: 1px solid white;
    border-radius: 4px;
    padding: 8px 12px;
}
.cell-button {
    width: 100%;
    height: 100%;
}
.x-color {
    color: green;
}
.o-color {
    color: red;
}
</style>
