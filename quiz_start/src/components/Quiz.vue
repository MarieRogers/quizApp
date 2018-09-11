<template>
    <div class="quiz" v-if="quiz.show" :style="{ 'background-color': quiz.color, 'color' : quiz.font}">
        <h1> {{quiz.name}} </h1>

        <questions
                   :question="quiz.questions[current]"
                   :key="quiz.questions[current].question"
                   :current="current"
                   :misses="misses"
                   :correct="correct"
                   :disabled="disabled"
                   :submit="submit"
                   v-on:submit-update="submit = !submit"
                   v-on:disabled-change="disabled = $event"
                   v-on:current-up="current = $event"
                   v-on:current-down="current = $event"
                   v-on:missed="misses.push($event)"
                   v-on:correct="correct.push($event)"
                   v-on:changed="misses.splice($event, 1)"
                   v-on:index="$emit('indexTop')">
        </questions>
    </div>
</template>

<script>

    import Questions from './Questions.vue'

    export default {
        name: 'quiz',
        props: ['quiz', 'idx'],
        components: {
            Questions
        },
        data () {
            return {
                count: 0,
                current: 0,
                front: 0,
                submit: true,
                misses: [],
                correct: [],
                disabled: true
            }
        }  
    };

</script>

<style>
    .quiz {
        padding: 10px;
    }
</style>