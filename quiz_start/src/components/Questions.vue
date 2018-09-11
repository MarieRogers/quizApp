<template>
    <div>
        <form>
            <div v-if="this.final">
                <h3>{{question.question}}</h3>
                <div v-for="(choice, index) in question.choices">
                    <input type="radio" class="radio" v-model="selected" :value="choice.value" @click="question.selected=choice.value">
                    <label :for="choice.value"> {{choice.value}} </label>
                </div>
                <label>Selected: {{question.selected}}</label>
                <br>
                <button value="Back" v-if="this.final" v-on:click.prevent="goBack(current, misses)" :disabled="Dis(current, misses)">Back</button>
                <button value="Submit" v-if="this.final" @click.prevent="submit(current, misses, question)">Submit</button>
                <button :disabled="this.ndis" v-if="current != 4 && this.final" value="Next"  @click.prevent="next(question, current, misses, correct)">Next</button>
            </div>
            <label class="sc" v-if="!this.final" value="this.score">{{this.score}}</label>
            <br>
            <button class="subButton" v-if="!this.final" @click="toIndex" v-cloak>New Quiz</button>
        </form>
    </div>
</template>

<script>

export default {
    name: 'questions',
    props: ['question','current', 'misses', 'correct'],
    data () {
        return {
            selected: null,
            backTo: 0,
            forwardTo : 0,
            ndis: true,
            final: true,
            score: ''
        };
    },
    mounted: function() {
        if(this.misses.length > 0) {
            this.$emit('disabled-change');
        }
    },
    methods: {
        //display back button?
        Dis: function(current, misses) {
            var min = 5;
            for(var x of misses) {

                if(x<min) min = x;
            }
            if(current > min) {
                return false;
            }
            return true;
        },
        //brings up index of buttons
        toIndex: function() {
            this.$emit('index');
        },
        //to next unanswered/incorrect question
        next: function(question, current, misses, correct) {
            var jump;
            if(this.forwardTo > current +1) {
                for(var miss in misses.reverse()) {
                    if(miss > current) {
                        jump = miss;
                    }
                }
                this.$emit('current-up', jump);
            }
            var cmax = 0;
            var mmax = 0;
            for(var x of correct) {

                if(x>cmax) cmax = x;
            }
            for(var y of misses) {
                if(y>mmax) mmax = y;
            }
            var max = Math.max(cmax, mmax);
            this.forwardTo = max +1;
            this.$emit('current-up', this.forwardTo)  ;
        },
        //submit a question/quiz
        submit: function(current, misses, question) {
            this.ndis = false;
            var idx = misses.indexOf(current);
            if(idx > -1) {
                this.$emit('changed', idx);
            }
            if(this.selected==question.correctAnswer) {
                this.$emit('correct', current);
            }
            if(this.selected!=question.correctAnswer) {
                this.$emit('missed', current);  
            }
            if(current == 4) {
                this.score = 'Your score is ' + (5 - misses.length) + '/5';
                this.final = !this.final;
            }
        },
        //go back to an incorrect question
        goBack: function(current, misses) {
            for(var miss of misses) {

                if(current > miss) {
                    this.backTo = miss;                    
                }
            }
            this.$emit('current-down', this.backTo);
        }
    }
};

</script>

<style>
.subButton {
    margin: 10px;
    font-size: 20pt;
    color: #2c3e50;
    border-radius: 8px;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
}
.sc {
    font-size: 20pt;
    padding: 10px;
}
</style>