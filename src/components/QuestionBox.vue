<template>
    <div>
        <b-container>
            <h2>Question {{ questionCount }}/{{ totalNumQuestions }}</h2>
            <b-row>
                <b-col sm="8" offset-sm="2" md="6" offset-md="3">
                    <h3 v-html="currentQuestion.question"></h3>
                    <b-list-group class="mt-3">
                        <b-list-group-item v-for="(answer, index) in answers"
                            :key="index"
                            @click="selectAnswer(index)"
                            :class="[
                                selectedIndex === index ?
                                'selected' :
                                ''
                            ]">
                            {{ answer }}
                        </b-list-group-item>
                    </b-list-group>
                    <b-row>
                        <b-col sm="6">
                            <b-button variant="primary"
                                class="mt-3"
                                :disabled="selectedIndex === null || answered"
                                @click="submitAnswer(); nextQuestion();"
                                v-if="lastQuestion">
                                Submit
                            </b-button>
                        </b-col>
                        <b-col sm="6">
                            <b-button variant="secondary"
                                class="mt-3"
                                @click="nextQuestion"
                                :disabled="lastQuestion() || !answered">
                                Next
                            </b-button>
                        </b-col>
                    </b-row>
                </b-col>
            </b-row>
        </b-container>
        <b-modal ref="completed-modal" hide-footer>
            <div class="d-block text-center">
                <h3>Quiz Completed!</h3>
                <p>You scored {{ correctAnswers }} out of {{ totalNumQuestions }} answers correctly</p>
                <b-button variant="primary" @click="restartQuiz">Restart</b-button>
            </div>
        </b-modal>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    data() {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            correctIndex: null,
            correctAnswers: 0,
            answered: false
        }
    },
    props: {
        currentQuestion: Object,
        questions: Array,
        nextQuestion: Function,
        totalNumQuestions: Number,
        questionCount: Number
    },
    computed: {
        answers() {
            return this.shuffledAnswers;
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null;
                this.answered = false;
                this.shuffleAnswers();
            }
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index;
        },
        shuffleAnswers() {
            let sAnswers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(sAnswers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        submitAnswer() {
            if (this.selectedIndex === this.correctIndex) {
                this.correctAnswers++;
            }
            this.answered = true;
            this.lastQuestion();
        },
        lastQuestion() {
            if (this.currentQuestion.question === this.questions[this.questions.length - 1].question) {
                return true;
            }
            return false;
        },
        completeQuiz() {
            if (this.lastQuestion()) {
                this.$refs['completed-modal'].show();
            }
        },
        restartQuiz() {
            location.reload();
        }
    }
}
</script>

<style scoped>
    h2 {
        font-size: 1.75rem;
    }

    h3 {
        font-size: 1.4rem;
        margin-bottom: 40px;
    }

    .list-group-item.selected {
        background: #aae5e2;
    }

    .list-group-item {
        background: #fff;
        margin-bottom: 15px;
        border-radius: 5px;
        cursor: pointer;
        color: #34607f;
        box-shadow: 0px 3px 15px rgba(0,0,0,0.2);
    }

    .list-group-item:first-child {
        border-top-left-radius: 5px;
        border-top-right-radius: 5px;
    }

    .list-group-item:last-child {
        border-bottom-left-radius: 5px;
        border-bottom-right-radius: 5px;
    }

    /deep/ .modal-backdrop {
        opacity: 0.3;
    }

    /deep/ .modal-body {
        padding: 2rem;
    }

    .btn {
        width: 200px;
        line-height: 2;
        border-radius: 25px;
        font-size: 18px;
        font-weight: 600;
    }

    .container .btn-primary,
    .container .btn-seconary {
        width: 100%;
    }
</style>