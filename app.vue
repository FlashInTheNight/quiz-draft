<template>
    <main
        class="bg-slate-800 w-screen h-screen flex justify-center items-center text-white">
        <div
            class="h-[600px] w-[600px] border-2 rounded-2xl border-stone-100 drop-shadow-2xl shadow-slate-100 p-4">
            <div
                v-if="toggleTest"
                id="questions"
                class="flex flex-col items-center justify-between h-full">
                <p class="text-white text-2xl text-center">
                    {{ data[currId].question }}
                </p>
                {{ selectedValue }}
                <div class="flex flex-col">
                    <label
                        :class="{
                            'text-green-400':
                                selectedValue !== '' &&
                                index === data[currId].answer,
                            'text-red-400':
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
                    id="next"
                    @click="nextQuestion"
                    v-if="selectedValue !== '' && currId < count">
                    next question
                </button>
                <button
                    id="finish"
                    @click="showFinish"
                    v-else-if="selectedValue !== '' && currId === count">
                    finish
                </button>
            </div>
            <div v-else id="result">
                <div>
                    <p>Кол-твло Правильных ответов</p>
                    {{ correctAnswers }}
                </div>
                <div>
                    <p>Кол-твло Неправильных ответов</p>
                    {{ wrongAnswers }}
                </div>
                <button @click="reset">reset</button>
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
        question: "hui budesh???",
        variants: { a: "da", b: "net", c: "ne znau", d: "sam hui" },
        answer: "d",
    },
    {
        question: "V chem ranitsa mejdu utkoi???",
        variants: {
            a: "a???",
            b: "da cht za huina",
            c: "ne znau",
            d: "sam pizda",
        },
        answer: "b",
    },
    {
        question: "god vipuska green elephant???",
        variants: { a: "1932", b: "24453", c: "1883", d: "1999" },
        answer: "d",
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
