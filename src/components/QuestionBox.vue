<template>
    <div class="question-box-container">
        <div>
            <b-jumbotron>
                <template slot="lead">
                    {{cleanQuestionString}}
                </template>
                    <hr class="my-4">
                    <b-list-group>
                        <b-list-group-item
                        v-for ="(answer, index) in shuffledAnswers" :key="index"
                        @click="selectAnswer(index)"
                        :class="answerClass(index)"
                        >
                        {{ answer }}
                        </b-list-group-item>
                    </b-list-group>
                    <b-button variant="primary"
                        @click="submitAnswer"
                        :disabled="selectedIndex === null || answered"
                       >
                        Submit
                        </b-button>
                    <b-button variant="success"
                    @click="next"
                    >
                    Next
                    </b-button>
            </b-jumbotron>
        </div>
    </div>
</template>


<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
    },
    computed:{
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers];
             answers.push(this.currentQuestion.correct_answer)
             return answers;
        },
        cleanQuestionString() {
            let str = this.currentQuestion.question
            let string_split = str.replace(/&quot;/g, "")
            let cleanQuestion = string_split.replace(/&#039;/g, "")
            return cleanQuestion;
        }
    },
    methods: {
        //this methods picks the index of a single answer array when the component is clicked and stores it to the data selectedIndex
        selectAnswer(index){
           this.selectedIndex = index; 
        },
        shuffleAnswers () {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer() {
           let isCorrect = false
           if (this.selectedIndex === this.correctIndex){
            isCorrect = true
           }
           this.answered =true
           this.increment(isCorrect)
        },
        answerClass(index){
        let answerClass =""
        if (! this.answered && this.selectedIndex === index) {
            answerClass = 'selected'
        }else if (this.answered && this.correctIndex === index) {
            answerClass = 'correct'
        }else if( this.answered && this.selectedIndex === index && this.correctIndex !==index) {
           answerClass ='incorrect' 
        }
        return answerClass
    }  
    },
    data (){
        return {
         correctIndex: null,
         selectedIndex : null,
         shuffledAnswers: [],
         answered: false
        }
    },
    watch:{
        //this watches for changes in the cuurentQuestion object in the props after which it sets selectedindex to null and run the function shuffleAnswers()
        currentQuestion: {
            immediate: true,
            handler(){
                this.answered = false
                this.selectedIndex = null;
                this.shuffleAnswers(); 
            }
             
        }
    },
}

</script>


<style scoped>
.list-group{
    margin-bottom: 15px;
}

.list-group-item:hover{
    background: #bbb;
    cursor: pointer;
}
.btn {
    margin:0px 5px;
}

.selected {
    background-color: lightblue;
}

.correct{
    background-color: lightgreen;
}

.incorrect{
    background-color: red;
}
</style>