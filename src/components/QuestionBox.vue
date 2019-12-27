<template>
    <div>
        <div class="jumbotron">
            <h1 class="display-5 lead">{{currentQuestion.question}}</h1>            
            <hr class="my-4">
            <ul class="list-group mb-4">
                <li class="list-group-item list-group-item-action" 
                    @click="selectAnswer(index)"
                    :class="answerClass(index)"
                    v-for="(answer,index) in shuffledAnswers" :key="index">
                    {{answer}}  
                </li>
            </ul>
            <button class="btn btn-primary btn-lg mr-4"             
            :disabled="selectedIndex === null || answered"
            @click="submitAnswer"             
            role="button">Submit</button>
            <button class="btn btn-primary btn-lg" 
            @click="next"  role="button">Next</button>
        </div>
    </div>
</template>
<script>
import _ from 'lodash'

export default {
    props:{
        currentQuestion:Object,
        next:Function,
        increment: Function
    },
    data(){
        return {
            selectedIndex: null,
            shuffledAnswers : [],
            correctIndex:null,
            answered:false
        }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index;
        },
        shuffleAnswers(){
            let answers = [... this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        submitAnswer(){
            let isCorrect = false;
            if(this.selectedIndex === this.correctIndex){
                isCorrect = true;
            }
            this.answered = true;
            this.increment(isCorrect);
        },
        answerClass(index){
            let answerClass = '';
             if(!this.answered && this.selectedIndex === index){
                 answerClass = 'selected';
             }
             else  if(this.answered && 
                this.correctIndex === index){
                    answerClass = 'correct';
             }
             else if(this.answered && 
                    this.selectedIndex === index &&
                    this.correctIndex !== index){
                 answerClass = 'incorrect';
             }

             return answerClass;
        }
    },
    watch:{
        currentQuestion:{
            immediate: true,
            handler(){
                this.selectedIndex = null;
                this.shuffleAnswers();
                this.answered = false;
            }
        }
    },
    computed:{
        answers(){
            let answers = [... this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);            
            return answers;
        }
    }
}
</script>

<style scoped>
.list-group-item:hover{
    background: #EEE;
    cursor: pointer;
}
.selected{
    background-color: lightblue;
}
.correct{
    background-color: lightgreen;
}
.incorrect{
    background-color: red;
}

</style>