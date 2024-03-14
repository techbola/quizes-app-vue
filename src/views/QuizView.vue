<script setup>
    import { computed, ref, watch } from "vue";
    import { useRoute } from "vue-router";
    import quizes from "../data/quizes.json";
    import Question from "@/components/Question.vue";
    import QuizHeader from "@/components/QuizHeader.vue";
    import Results from "@/components/Result.vue"

    const route = useRoute();

    const quizId = parseInt(route.params.id);

    const quiz = quizes.find((q) => q.id === quizId);

    const currentQuestionIndex = ref(0);
    const numberOfCorrectAnswers = ref(0);
    const showResults = ref(false);

    // const questionStatus = ref(`${currentQuestionIndex.value}/${quiz.questions.length}`);

    // watch(currentQuestionIndex, () => {
    //     questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`;
    // });

    const questionStatus = computed(() => {
        return `${currentQuestionIndex.value}/${quiz.questions.length}`;
    });

    const barPercentage = computed(() => {
        return `${currentQuestionIndex.value/quiz.questions.length * 100}%`;
    });

    const onOptionSelected = (isCorrect) => {
        if (isCorrect) {
            numberOfCorrectAnswers.value++;
        }
        showNextQuestion();
    }

    const showNextQuestion = () => {
        if (currentQuestionIndex.value === quiz.questions.length - 1) {
            showResults.value = true;
        }
        currentQuestionIndex.value++;
    }

</script>

<template>
    <div>
        <QuizHeader :questionStatus="questionStatus" :barPercentage="barPercentage" />
        <div v-if="!showResults">
            <div>
                <Question 
                    :question="quiz.questions[currentQuestionIndex]" 
                    @selectOption="onOptionSelected"
                />
            </div>
            <!-- <button @click="showNextQuestion()">Next Question</button> -->
        </div>
        <Results
            v-else 
            :quizQuestionLength="quiz.questions.length"
            :numberOfCorrectAnswers="numberOfCorrectAnswers"
        />
    </div>
</template>

<style scoped>

</style>
