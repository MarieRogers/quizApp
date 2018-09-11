<template>
    <div id="app" class="container">
        <div class="page-header">
            <h1 id="title">Quiz</h1>
        </div>
        <div v-if="this.button" >
            <div v-for="quiz in data1">
                <button  class="quizChoice" value="quiz.name" @click="Show(quiz)">{{quiz.name}}</button>
            </div>
        </div>
        <quiz v-for="(quiz, index) in data1" 
              :quiz="quiz"
              :key="quiz.name"
              :idx="index"
              v-on:indexTop="button = true">
        </quiz>
    </div>
</template>

<script>

    import Quiz from './components/Quiz.vue'
    import data1 from './assets/data/data1.json'

    export default {
        name: 'app',
        components: {
            Quiz
        },
        data () {
            return {
                data1: data1,
                button: true
            }
        },
        methods: {
            //show/hide quiz
            Show: function(quiz) {
                quiz.show = !quiz.show;
                this.button = false;
            }
        }
    };

</script>


<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    h1, h2 {
        font-weight: normal;
    }

    #title {
        font-size: 40pt;
    }
    .quizChoice {
        margin: 5px;
        font-size: 20pt;
        color: #2c3e50;
        border-radius: 8px;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
    }
</style>
