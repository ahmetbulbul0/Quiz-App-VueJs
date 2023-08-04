<template>
  <main class="app">
    <h1>The Quiz App</h1>
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }}/{{ questions.length }}</span>
      </div>
      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.options" :key="index" :class="`option ${getCurrentQuestion.selected == index
            ? index == getCurrentQuestion.answer
              ? 'correct'
              : 'wrong'
            : ''
          } ${getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
            ? 'disabled'
            : ''
          }`">
          <input type="radio" :name="getCurrentQuestion.index" :value="index" v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected" @change="SetAnswer" />
          <span>{{ option }}</span>
        </label>
      </div>

      <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index == questions.length - 1
          ? "Finish"
          : getCurrentQuestion.selected == null
            ? "Select An Option"
            : "Next Question"
        }}
      </button>
    </section>

    <section v-else>
      <h2>You Have Finished The Quiz!</h2>
      <p>Your Score Is {{ score }} / {{ questions.length }}</p>
    </section>
  </main>
</template>

<script setup>
import { ref, computed } from "vue";

const questions = ref([
  {
    question:
      "Id amet aliquip commodo adipisicing eiusmod ex qui labore reprehenderit nostrud.",
    answer: 0,
    options: [
      "Dolor voluptate cupidatat pariatur voluptate ullamco tempor Lorem anim veniam.",
      "Voluptate duis ex dolore minim occaecat cillum fugiat esse nulla proident consectetur proident amet aute.",
      "Elit pariatur ullamco excepteur deserunt veniam quis do non tempor irure.",
    ],
    selected: null,
  },
  {
    question: "Consectetur ex Lorem cillum proident reprehenderit.",
    answer: 2,
    options: [
      "Reprehenderit consectetur anim ad proident proident.",
      "Deserunt laborum est veniam aute culpa cillum elit amet enim consectetur ea labore aute commodo.",
      "Ex irure incididunt aliqua anim anim proident nisi duis magna et.",
    ],
    selected: null,
  },
  {
    question: "Mollit excepteur consectetur nostrud anim amet occaecat.",
    answer: 1,
    options: [
      "Duis pariatur consequat aute nostrud dolore velit eiusmod sit magna excepteur.",
      "Ullamco fugiat minim fugiat tempor commodo ex id aute excepteur exercitation cillum ipsum cillum.",
      "Nisi mollit Lorem occaecat cillum velit.",
    ],
    selected: null,
  },
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected == q.answer) {
      value++;
    }
  });
  return value;
});

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});

const SetAnswer = (evt) => {
  questions.value[currentQuestion.value].selected = evt.target.value;
  evt.target.value = null;
};

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
  } else {
    quizCompleted.value = true;
  }
};
</script>