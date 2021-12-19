<template>
    <div :style="{backgroundColor: color}" class="pb-5">
        <b-container>
            <b-row>
                <b-col md="6" offset-md="3">
                    <div class="question-box">
                        <header class="mb-3 text-left">
                            <span>Question {{ questionCount }}.</span>
                        </header>
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
                                <span v-html="answer"></span>
                            </b-list-group-item>
                        </b-list-group>
                        <b-row>
                            <b-col sm="12">
                                <b-button variant="primary"
                                    class="mt-3"
                                    :disabled="selectedIndex === null || answered"
                                    @click="submitAnswer(); nextQuestion();"
                                    v-if="lastQuestion">
                                    Submit
                                </b-button>
                            </b-col>
                            <!-- <b-col sm="6">
                                <b-button variant="secondary"
                                    class="mt-3"
                                    @click="nextQuestion"
                                    :disabled="lastQuestion() || !answered">
                                    Next
                                </b-button>
                            </b-col> -->
                        </b-row>
                    </div>
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
            answered: false,
            color: '#124DF0',
        }
    },
    props: {
        currentQuestion: Object,
        questions: Array,
        nextQuestion: Function,
        totalNumQuestions: Number,
        questionCount: Number,
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
    h3 {
        font-size: 1.4rem;
        margin-bottom: 40px;
    }

    .list-group-item.selected {
        border-color: #124DF0;
        color: #124DF0;
    }

    .list-group-item {
        background: #fff;
        margin-bottom: 15px;
        border: 2px solid #ececee;
        border-radius: 25px;
        cursor: pointer;
    }

    .list-group-item:first-child {
        border-top-left-radius: 25px;
        border-top-right-radius: 25px;
    }

    .list-group-item:last-child {
        border-bottom-left-radius: 25px;
        border-bottom-right-radius: 25px;
    }

    .list-group-item + .list-group-item {
        border-top-width: 2px;
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

    .question-box {
        background: #fff;
        padding: 20px 20px 40px;
        border-radius: 25px;
        box-shadow: 0px 3px 15px rgba(0,0,0,0.2);
    }

    header > span {
        font-size: 20px;
        font-weight: 700;
    }
</style>