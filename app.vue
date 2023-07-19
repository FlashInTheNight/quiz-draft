<template>
    <main
        class="bg-slate-800 w-screen h-screen flex justify-center items-center text-white">
        <div
            class="h-[600px] w-[600px] border-2 rounded-2xl border-stone-100 drop-shadow-2xl shadow-slate-100 p-4">
            <div
                v-if="toggleTest"
                id="questions"
                class="flex flex-col items-center gap-y-10 h-full">
                <p class="text-white sm:text-2xl text-center">
                    {{ data[currId].question }}
                </p>
                <div class="flex flex-col gap-y-5 w-full">
                    <label
                        class="border-2 rounded p-2 cursor-auto text-center"
                        :class="{
                            'cursor-pointer': selectedValue === '',
                            'text-neutral-100 bg-green-400 border-green-400':
                                selectedValue !== '' &&
                                index === data[currId].answer,
                            'bg-red-400 text-neutral-100 border-red-400':
                                selectedValue !== '' &&
                                index === selectedValue &&
                                index !== data[currId].answer,
                        }"
                        :for="index"
                        v-for="(item, index) in data[currId].variants"
                        :key="item"
                        @change="checkAnswer(index)">
                        <input
                            class="hidden"
                            :disabled="selectedValue !== ''"
                            :name="currId"
                            :id="index"
                            type="radio" />
                        {{ item }}
                    </label>
                </div>
                <button
                    class="mt-auto border-2 py-1 px-2 rounded"
                    id="next"
                    @click="nextQuestion"
                    v-if="selectedValue !== '' && currId < count">
                    next question
                </button>
                <button
                    class="mt-auto border-2 py-1 px-2 rounded"
                    id="finish"
                    @click="showFinish"
                    v-else-if="selectedValue !== '' && currId === count">
                    finish
                </button>
            </div>
            <div
                v-else
                class="flex flex-col items-center justify-center gap-y-10 h-full text-center"
                id="result">
                <div class="sm:text-2xl lg:text-3xl mt-24">
                    <p>Кол-во Правильных ответов</p>
                    <span class="text-red-400">{{ correctAnswers }}</span>
                </div>
                <div class="sm:text-2xl lg:text-3xl">
                    <p>Кол-во Неправильных ответов</p>
                    <span class="text-green-400">{{ wrongAnswers }}</span>
                </div>
                <button
                    class="mt-auto border-2 py-1 px-2 rounded"
                    @click="reset">
                    reset
                </button>
            </div>
        </div>
    </main>
</template>

<script setup>
const toggleTest = ref(true);

let currId = ref(0);
let selectedValue = ref("");
let count = ref(2);

let correctAnswers = ref(0);
let wrongAnswers = ref(0);

const data = reactive([
    {
        question: "Eighteen thousandths, written as a decimal, is:",
        variants: { a: "0.0018", b: "0.018", c: "0.18", d: "0.000018" },
        answer: "a",
    },
    {
        question: "The next number in the sequence 1, 3, 6, 10, is:",
        variants: {
            a: "12",
            b: "13",
            c: "14",
            d: "15",
        },
        answer: "d",
    },
    {
        question: "JSON stands for",
        variants: {
            a: "Java Standard Output Network",
            b: "JavaScript Object Notation",
            c: "avaScript Output Name",
            d: "Java Source Open Network",
        },
        answer: "b",
    },
]);

console.log(data[currId.value].answer);
function checkAnswer(index) {
    selectedValue.value = index;
    if (index === data[currId.value].answer) {
        correctAnswers.value++;
    } else {
        wrongAnswers.value++;
    }
}

function nextQuestion() {
    currId.value++;
    selectedValue.value = "";
}

function showFinish() {
    toggleTest.value = !toggleTest.value;
}

function reset() {
    toggleTest.value = !toggleTest.value;
    currId.value = 0;
    selectedValue.value = "";
    wrongAnswers.value = 0;
    correctAnswers.value = 0;
}
</script>
