<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template #header></template>
                {{currentQuestion.question}}

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item v-for="(shuffledAnswers, index) in shuffledAnswers" :key="index"
                @click="selectAnswer(index)"
                :class="answerClass(index)">
                    {{shuffledAnswers}}
                </b-list-group-item>
            </b-list-group>

            <b-button class="b-button" variant="primary"
            @click="submitAnswer()"
            :disabled="selectedIndex === null || answered === false || submitted === true">
            Submit
            </b-button>
            <b-button class="b-button" variant="success"  @click="next()">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash';

export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
    },
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false,
            submitted: false,
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);
            return answers;
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index;
            this.answered = true;
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        submitAnswer() {
            let isCorrect = false;

            if(this.selectedIndex === this.correctIndex) {
                isCorrect = true;
            }

            this.submitted = true;
            this.answered = false;
            this.increment(isCorrect);
        },
        answerClass(index) {
            let answerClass = '';

            if(!this.answerClass &&  this.selectedIndex === index) {
                answerClass = 'selected'
            } else if (this.answered && this.correctIndex === index) {
                answerClass = 'true-answer'
            } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
                answerClass = 'wrong-answer'
            }

            return answerClass;
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null;
                this.answered = false;
                this.submitted = false;
                this.shuffleAnswers();
            }
        }
    }
}
</script>

<style scoped>
.list-group {
    margin-bottom: 10px;
}

.list-group-item:hover {
    cursor: pointer;
    background: rgb(190, 186, 186);
    color: white;
}

.selected {
    background-color: rgb(109, 109, 230);
    color: white;
}

.true-answer {
    background-color: rgb(130, 245, 130);
    color: white;
}

.wrong-answer {
    background-color: rgb(243, 57, 57);
    color: white;
}

.b-button {
    margin: 0px 5px;
}
</style>
