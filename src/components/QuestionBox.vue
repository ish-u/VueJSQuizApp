<template>
    <div class="question-box-container">
        <b-jumbotron>
            <h3 v-html="currentQuestion.question"></h3>
            <hr class="my-4">
            <b-list-group>
                <b-list-group-item 
                    v-html="answer"
                    v-for="(answer,index) in shuffledAnswers" :key="index"
                    @click="selectAnswer(index)"
                    :class="answerClass(index)"
                >
                </b-list-group-item>
            </b-list-group>
            <b-button variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >
                Submit
            </b-button>
            <b-button @click="next" variant="success" href="#">Next Question</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash';
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data() {
        return{
            selectedIndex: null,
            correctIndex: null,
            answered: false,
            shuffledAnswers: []
        }
    },
    watch:{
        currentQuestion: {
            immediate : true,
            handler(){
                this.selectedIndex = null;
                this.shuffleAnswers();
                this.answered = false;
            } 
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer);
            return answers; 
        }
    },
    methods : {
        selectAnswer(index){
            this.selectedIndex = index;
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        submitAnswer() {
            let isCorrect = false;
            if(this.selectedIndex === this.correctIndex){
                isCorrect = true;
            }
            this.answered = true
            this.increment(isCorrect);
        },
        answerClass(index) {
            let answerClass = '';
            if(!this.answered && this.selectedIndex === index)
            {
                answerClass = 'selected';
            } 
            else if(this.answered && this.correctIndex === index)
            {
                answerClass = 'correct'
            }
            else if(this.answered && this.selectedIndex === index && this.correctIndex != index)
            {
                answerClass = 'incorrect'
            }
            return answerClass;
        }
    },

}
</script>

<style scoped>
    .question-box-container{
        padding: 20px;
    }
    .list-group{
        margin-bottom: 15px;
    }
    .list-group-item:hover{
        background: #EEE;
        cursor: pointer;
    }
    .btn{
        margin: 0 5px;
    }
    .selected{
        background-color: aquamarine;
    }
    .correct{
        background-color: lightgreen;
    }
    .incorrect{
        background-color: rgba(230, 21, 21, 0.823);
    }
</style>