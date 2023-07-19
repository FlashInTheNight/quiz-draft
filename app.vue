<template>
  <main class="bg-slate-800 w-screen h-screen flex justify-center items-center">
    <div class="h-[600px] w-[600px] border-2 rounded-2xl border-stone-100 drop-shadow-2xl shadow-slate-100 p-4">
      <div v-if="toggleTest" id="questions" class="flex flex-col items-center justify-between h-full">
        <p class="text-white text-2xl text-center">
          {{ data[currId].question }}
        </p>
        {{ selectedValue }}
        <div class="flex flex-col">
          <label class="text-white" :class="{
            'text-green-400': selectedValue !== '' && index === data[currId].answer,
            'text-red-400': selectedValue !== '' && index === selectedValue && index !== data[currId].answer
          }" :for="index" v-for="(item, index) in data[currId].variants" :key="item" @click="checkAnswer(index)">
            <input :name=currId :id="index" type="radio">
            {{ item }}
          </label>
        </div>
      </div>
      <div v-else id="result">hui</div>
    </div>
  </main>
</template>

<script setup>
const toggleTest = ref(true);

let currId = ref(0);
let selectedValue = ref("");
let count = ref(3);

let correctAnswers = ref(0)
let wrongAnswers = ref(0)

const data = reactive([
  {
    question: "hui budesh???",
    variants: { a: "da", b: "net", c: "ne znau", d: "sam hui" },
    answer: "d",
  },
  {
    question: "V chem ranitsa mejdu utkoi???",
    variants: { a: "a???", b: "da cht za huina", c: "ne znau", d: "sam pizda" },
    answer: "b",
  },
  {
    question: "god vipuska green elephant???",
    variants: { a: "1932", b: "24453", c: "1883", d: "1999" },
    answer: "d",
  },
]);

function checkAnswer(index) {
  selectedValue.value = index;
  if (selectedValue === data[currId.value].answer) {
    correctAnswers.value++
  } else {
    wrongAnswers.value++
  }

}
</script>
