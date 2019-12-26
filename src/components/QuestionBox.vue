<template>
    <div>
        <div class="jumbotron">
            <h1 class="display-5 lead">{{currentQuestion.question}}</h1>            
            <hr class="my-4">
            <ul class="list-group mb-4">
                <li class="list-group-item list-group-item-action" 
                    @click="selectAnswer(index)"
                    :class="[selectedIndex === index ? 'selected' : '']"
                    v-for="(answer,index) in answers" :key="index">
                    {{answer}}  
                </li>
            </ul>
            <a class="btn btn-primary btn-lg mr-4" href="#" role="button">Submit</a>
            <a class="btn btn-primary btn-lg" @click="next" href="#" role="button">Next</a>
        </div>
    </div>
</template>
<script>
import _ from 'lodash'

export default {
    props:{
        currentQuestion:Object,
        next:Function
    },
    data(){
        return {
            selectedIndex: null,
            shuffledAnswers : []
        }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index;
        },
        shuffleAnswers(){
            let answers = [... this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers);
        }
    },
    watch:{
        currentQuestion:{
            immediate: true,
            handler(){
                this.selectedIndex = null;
                this.shuffleAnswers();
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