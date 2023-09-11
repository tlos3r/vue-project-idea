<template>
    <h1 class="pt-20 text-5xl font-bold text-center">Math Quiz App Random</h1>

    <!-- Selector -->
    <div v-if="!operator" class="flex items-center justify-center mt-10 text-center">
        <button @click="operatorChoose('+')" class="px-5 py-5 mr-5 text-xl text-white rounded-lg bg-zinc-500">
            Addition
        </button>
        <button @click="operatorChoose('-')" class="px-5 py-5 mr-5 text-xl text-white rounded-lg bg-zinc-500">
            Subtraction
        </button>
        <button @click="operatorChoose('*')" class="px-5 py-5 mr-5 text-xl text-white rounded-lg bg-zinc-500">
            Multiplication
        </button>
        <button @click="operatorChoose('/')" class="px-5 py-5 text-xl text-white rounded-lg bg-zinc-500">
            Division
        </button>
    </div>

    <!-- Quiz -->
    <div v-else class="flex justify-center mt-10 flex-[1_0_50%] flex-wrap flex-row">
        <div v-if="isStarted" class="flex-[0_1_100%] text-center text-2xl my-5">
            <h2>{{ numLeft }} {{ operator }} {{ numRight }}</h2>
            <button
                v-for="(ans, index) in answers"
                :key="index"
                class="px-5 py-5 mt-5 mr-5 text-xl text-white rounded-lg bg-zinc-500"
                @click="selectAnswer(ans)"
            >
                {{ ans }}
            </button>
        </div>
        <button v-else @click="startQuiz" class="px-5 py-5 mt-5 mr-5 text-xl text-white rounded-lg bg-zinc-500">
            Start
        </button>
        <button @click="clearAll" class="px-5 py-5 mt-5 text-xl text-white rounded-lg bg-zinc-500">Go back</button>
    </div>
</template>

<script setup lang="ts">
import { reactive, ref, type Ref } from "vue";
const operator: Ref<string | null> = ref("");
const numLeft: Ref<number | null> = ref(null);
const numRight: Ref<number | null> = ref(null);
const isStarted: Ref<boolean> = ref(false);
const expectedAnswer: Ref<number> = ref(0);
const answers: Ref<number[]> = ref([]);

const operatorChoose = (ope: string) => (operator.value = ope);

const clearAll = () => {
    operator.value = null;
    numLeft.value = null;
    numRight.value = null;
    isStarted.value = false;
};
const selectAnswer = (answerSelected: number) => {
    if (answerSelected === expectedAnswer.value) {
        startQuiz();
    } else {
        alert(`Choose wrong answer the correct is ${expectedAnswer.value}`);
        startQuiz();
    }
};
const startQuiz = () => {
    isStarted.value = true;
    numLeft.value = Math.floor(Math.random() * 51);
    numRight.value = Math.floor(Math.random() * 51);

    const methods = reactive({
        "+": (a: number, b: number) => a + b,
        "-": (a: number, b: number) => a - b,
        "*": (a: number, b: number) => a * b,
        "/": (a: number, b: number) => a / b,
    });

    answers.value = [];
    for (let i = 0; i < 4; i++) {
        let answer = methods[operator.value as keyof typeof methods](
            numLeft.value * Math.floor(Math.random() * 3),
            numRight.value * Math.floor(Math.random() * 3)
        );
        if (i >= 1 && answer === answers.value[i - 1]) {
            answer = methods[operator.value as keyof typeof methods](
                numLeft.value * Math.floor(Math.random() * 3),
                numRight.value * Math.floor(Math.random() * 3)
            );
        }
        answers.value.push(answer);
    }
    const rightAnswer = methods[operator.value as keyof typeof methods](numLeft.value, numRight.value);
    answers.value[Math.floor(Math.random() * answers.value.length)] = rightAnswer;
    expectedAnswer.value = rightAnswer;
};
</script>

<style scoped></style>
