<script setup>
import { ref, computed } from 'vue'

const questions = ref( [
    {
        question: 'What is Vue.js?',
        answer  : 0,
        options : [
            'A framework for building user interfaces and web app',
            'A library',
            'An application to render 3d models'
        ],
        selected : null
    },
    {
        question: 'What is Vuex?',
        answer  : 2,
        options : [
            'Vue with an x',
            'A cheese selection',
            'State Management library'
        ],
        selected : null
    },
    {
        question: 'What is Vue Router used for?',
        answer  : 1,
        options : [
            'Walking in space',
            'A routung library for Vue.js',
            'Burger sauce',
            'Quizzes'
        ],
        selected : null
    }
] )

const quizCompleted   = ref( false )
const currentQuestion = ref( 0 ) 
const score           = computed( () => {
    let value = 0
    questions.value.map( q => { 
        if (q.selected == q.answer) {
            value++
        }
    } )
    return value
} ) 

const getCurrentQuestions = computed( () => {
    let question   = questions.value[ currentQuestion.value ]
    question.index = currentQuestion.value
    return question
} )

const setAnswer = evt => {
    questions.value[currentQuestion.value].selected = evt.target.value
    evt.taget.value = null
}

const nextQuestion = () => {
    if( currentQuestion.value < questions.value.length - 1) {
        currentQuestion.value++
    } else {
        quizCompleted.value = true
    }
}

</script>

<template>
    <main class="app">
        <h1>The Quiz</h1>

        <section class="quiz" v-if="!quizCompleted">
            <div class="quiz-info">
                <span class="question">
                    {{ getCurrentQuestions.question }}
                </span>
                <span class="score">
                    Score {{ score }} / {{ questions.length }}
                </span>
            </div>
            <div class="options">
                <label v-for="(option, index) in getCurrentQuestions.options" 
                    :key="index" 
                    :class="`option ${
                        getCurrentQuestions.selected == index 
                            ? index == getCurrentQuestions.answer ? 'correct' : 'wrong'
                            : ''           
                    } ${
                        getCurrentQuestions.selected != null && 
                        index != getCurrentQuestions.selected
                        ? 'disabled'
                        : ''
                    }
                    `">
                    <input 
                        type="radio" 
                        :name="getCurrentQuestions.index"
                        :value="index"
                        v-model="getCurrentQuestions.selected"
                        :disabled="getCurrentQuestions.selected"
                        @change="setAnswer">
                        <span>{{ option }}</span>
                </label>
            </div>

            <button @click="nextQuestion" :disabled="!getCurrentQuestions.selected">
                {{ getCurrentQuestions.index == questions.length - 1
                        ? 'Finish'
                        : getCurrentQuestions.selected == null 
                            ? 'Select an option'
                            : 'Next Question'     
                }}
            </button>
        </section>

        <section v-else>
            <h2>You have finished the quiz.</h2>
            <p>Your score is {{ score }}/{{ questions.length }}</p>
        </section>
    </main>
</template>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Montserrat', sans-serif;
}

body {
    background-color: #271C36;
    color: #FFF;

}

.app {
    display:flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    min-height: 100vh;
}

h1 {
    font-size: 2rem;
    margin-bottom: 2rem;    
}

.quiz {
    background-color: #382a4b;
    padding: 1rem;
    width: 100%;
    max-width: 640px;
    border-radius: 0.5rem;
}

.quiz-info {
    display: flex;
    justify-content: space-between;
    margin-bottom: 1rem;
}

.quiz-info .question {
    color: #8f8f8f;
    font-size: 1.25rem;
}

.quiz-info .score {
    color: #FFF;
    font-size: 1.25rem;
}

.options {
    margin-bottom: 1rem;
}

.option {
    padding: 1rem;
    display: block;
    background-color: #271C35;
    margin-bottom: 0.5rem;
    border-radius: 0.5rem;
    cursor: pointer;
}

.option:hover {
    background-color: #2d213f;
}

.option.correct {
    background-color: #2cce7d;
}

.option.wrong {
    background-color: #ff5a5f;
}

.option:last-of-type {
    margin-bottom: 0;
}

.option.disabled {
    opacity: 0.5;
}

.options input {
    display: none;
}

button {
    appearance: none;
    outline: none;
    border: none;
    cursor: pointer;

    padding: 0.5rem 1rem;
    background-color: #2cce7d;
    color: #2d213f;
    font-weight: 700;
    text-transform: uppercase;
    font-size: 1.2rem;
    border-radius: 0.5rem;
}

buton:disabled {
    opacity: 0.5;
}

h2 {
    font-size: 2rem;
    margin-bottom: 2rem;
    text-align: center;
}

p {
    color: #8f8f8f;
    font-size: 1.25rem;
    text-align: center;
}
</style>
