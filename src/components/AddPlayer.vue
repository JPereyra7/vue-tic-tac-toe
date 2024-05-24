<script setup lang="ts">
import { defineEmits, ref } from 'vue';

const inputValue = ref('')
const emit = defineEmits(['addPlayer', 'startBtn']);

const isActive = ref(true);

const currentPlayer = ref('X');

const hideGame = ref(true)
const vShow = ref(false)
const registerBtn = ref(true)


const toggleFormClass = () => {
    isActive.value = !isActive.value;
}
const addingPlayer = () => {
    if(inputValue.value !== '') {
        emit('addPlayer', inputValue.value);
        currentPlayer.value = currentPlayer.value === "X" ? "O" : "✅" ;
        showAndHideBtn()
        inputValue.value = "";
    }
}

const startGame = () => {
 emit('startBtn', hideGame.value);
 toggleFormClass();

}

const showAndHideBtn = () => {
    if(currentPlayer.value === "✅") {
        vShow.value = true;
        registerBtn.value = false;
    } 
}

</script>

<template>
    <div :class="{ formContainerActive: isActive, formContainerInactive: !isActive }">
        <h2 class="registreraSpelareText">Registrera spelare {{ currentPlayer }} </h2>
        <form @submit.prevent="addingPlayer()">
            <input class="inputStyling" v-model="inputValue" type="text" >
            <button class="zeButtons" v-show="registerBtn" type="submit">Registrera</button>
        </form>
        
        <button class="zeButtons" v-show="vShow" @click="startGame">Starta Spelet</button>
    </div>
</template>

<style lang="scss" scoped>
input, ::placeholder {
    text-align: center;
    color: black;
    font-weight: bold;
    margin-bottom: 1rem;
}

button {
    
    display: block;
    margin: auto;
}

.formContainerInactive {
    display: none;
}

.registreraSpelareText {
    font-family: Poppins, sans-serif;
    font-weight: 600;
    letter-spacing: 2px;
    color: white;
    text-shadow: 
        0 0 7px #ffffff,
        0 0 1px #fff,
        0 0 42px rgb(0, 242, 255),
    }

    .inputStyling {
        border: none;
        background-color: transparent;
        color: #fff;
        border-bottom: 1px solid white;
        box-shadow: none;
        font-size: 1.05em;

        &:focus {
            outline: none;
            box-shadow: none;
        }
        &:active {
            outline: none;
            box-shadow: none;
            -webkit-box-shadow: none;
        }
    }

    .zeButtons {
        box-shadow: 
        0 0 7px #ffffff,
        0 0 1px #fff,
        0 0 42px rgb(0, 242, 255);
        margin-bottom: 2em;
        border: none;

        &:hover {
        transition: 0.8s;
        box-shadow: 
        0 0 7px #ffffff,
        0 0 1px #ffeacb,
        0 0 42px rgb(255, 166, 0);
        border: none;
        }
        &:active {
            outline: none;
            border: none;
        }
    }
    
</style>