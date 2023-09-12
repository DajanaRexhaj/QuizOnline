<script setup>
import { ref, computed } from 'vue'

const questions = ref ([
{
  question: 'When was John F.Kennedy assassinated?',
  category: 'HISTORY',
  options: [
    '1950',
    '1963',
    '1960',
    '1955'
  ],
  answer: 1,
  selected: null
},
{
  question: 'In which year was the Berlin Wall torn down?',
  category: 'HISTORY',
  options: [
    '1980',
    '1985',
    '1989',
    '1990'
  ],
  answer: 2,
  selected: null
},
{
  question: 'When was Pearl Harbor bombed?',
  category: 'HISTORY',
  options: [
    'December 1st 1941',
    'December 5th 1941',
    'December 6th 1941',
    'December 7th 1941'
  ],
  answer: 3,
  selected: null
},
{
  question: 'What was the name of the first computer?',
  category: 'HISTORY',
  options: [
    'ENIX',
    'ENIAC',
    'ENIAX',
    'ENIAC 1.0'
  ],
  answer: 1,
  selected: null
},
{
  question: 'What does DNA stand for?',
  category: 'SCIENCE',
  options: [
    'Deoxnucleic acid',
    'Dexyribonucleic acid',
    'Dexiornucleic acid',
    'Deoxyribonucleic acid'
  ],
  answer: 3,
  selected: null
},
{
  question: 'What metal is the best conductor of electricity?',
  category: 'SCIENCE',
  options: [
    'Copper',
    'Iron',
    'Aluminium',
    'Silver'
  ],
  answer: 3,
  selected: null
},
{
  question: 'Which number is a prime?',
  category: 'SCIENCE',
  options: [
    '2',
    '4',
    '6',
    '12'
  ],
  answer: 0,
  selected: null
},
{
  question: 'Which is the main gas that makes up the atmosphere in Earth?',
  category: 'SCIENCE',
  options: [
    'Oxygen',
    'Argon',
    'Nitrogen',
    'Methane'
  ],
  answer: 2,
  selected: null
},
{
  question: 'What is the name of the largest country in the world?',
  category: 'GEOGRAPHY',
  options: [
    'China',
    'Canada',
    'Russia',
    'USA'
  ],
  answer: 2,
  selected: null
},
{
  question: 'How many countries are there in the United Kingdom?',
  category: 'GEOGRAPHY',
  options: [
    '1',
    '2',
    '3',
    '4'
  ],
  answer: 3,
  selected: null
},
{
  question: 'What is the name of the largest island in the world?',
  category: 'GEOGRAPHY',
  options: [
    'Greenland',
    'Bell Island',
    'Cape Breton Island',
    'Haida Gwaii'
  ],
  answer: 0,
  selected: null
},
{
  question: 'Where is mount Fuij located?',
  category: 'GEOGRAPHY',
  options: [
    'China',
    'Thailand',
    'Malaysia',
    'Japan'
  ],
  answer: 3,
  selected: null
},
{
  question: 'Which of these movies is by Christopher Nolan?',
  category: 'ENTERTAINMENET',
  options: [
    'The Batman',
    'Memento',
    'Pulp Fiction',
    'Fight Club'
  ],
  answer: 1,
  selected: null
},
{
  question: 'Who is the creater of Dragon Ball?',
  category: 'ENTERTAINMENET',
  options: [
    'Masashi Kishimoto',
    'Eiichiro Oda',
    'Gege Akutami',
    'Akira Toriyama'
  ],
  answer: 3,
  selected: null
},
{
  question: 'Who won the Oscars Best Actor as Joker?',
  category: 'ENTERTAINMENET',
  options: [
    'Joaquin Phoenix',
    'Heath Ledger',
    'Barry Keoghan',
    'Jack Nicholson'
  ],
  answer: 0,
  selected: null
},
{
  question: 'Who is considered to be the best guitarist of all time?',
  category: 'ENTERTAINMENET',
  options: [
    'Jimmy Page',
    'Santana',
    'Jimi Hendrix',
    'Eric Clapton'
  ],
  answer: 2,
  selected: null
}

]);

const categories = computed(() => [...new Set(questions.value.map(question => question.category))]);
const selectedCategory = ref(null);
const currentQuestion = ref(0);
const quizCompleted = ref(false);

const filteredQuestions = computed(() => {
  if (selectedCategory.value === null) {
    return [];
  }
  return questions.value.filter(question => question.category === selectedCategory.value);
});

let score = ref(0);

const getCurrentQuestion = computed(() => {
  if (filteredQuestions.value.length === 0 || currentQuestion.value >= filteredQuestions.value.length) {
    return null;
  }
  let question = filteredQuestions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});
const SetAnswer = evt => {
  if (selectedCategory.value !== null) {
    filteredQuestions.value[currentQuestion.value].selected = evt.target.value;

    // Check if the selected answer is correct and update the score
    if (evt.target.value === filteredQuestions.value[currentQuestion.value].answer.toString()) {
      score.value++;
    }
  }
};

const NextQuestion = () => {
  if (selectedCategory.value !== null && currentQuestion.value < filteredQuestions.value.length - 1) {
    currentQuestion.value++;
  } else if (selectedCategory.value !== null) {
    quizCompleted.value = true;
  }
};

const selectCategory = category => {
  selectedCategory.value = category;
  currentQuestion.value = 0; 
  score.value = 0; 
};

const startAnotherQuiz = () => {
  selectedCategory.value = null; 
  currentQuestion.value = 0; 
  score.value = 0; 
  quizCompleted.value = false; 
};

</script>

<template>
  <main class="app">
    <h1>Online Quiz</h1>

    <div class="category-boxes" v-if="selectedCategory === null">
      <div
        v-for="category in categories"
        :key="category"
        class="category-box"
        @click="selectCategory(category)">

        {{ category }}

      </div>
    </div>

    <section class="quiz" v-if="selectedCategory !== null && !quizCompleted">
      <div class="quiz-info">
        <span class="question">
          {{ getCurrentQuestion.question }}
        </span>
        <span class="score">Score {{ score }}/{{ filteredQuestions.length }}</span>
      </div>

      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.options" :key="index"
          :class="`option
        ${getCurrentQuestion.selected == index ? index == getCurrentQuestion.answer ? 'correct' : 'wrong' : ''
          } ${
            getCurrentQuestion.selected != null && index != getCurrentQuestion.selected ? 'disabled' : ''
          }`">

          <input type="radio" :name="getCurrentQuestion.index" :value="index" v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected" @change="SetAnswer">

          <span> {{ option }} </span>
        </label>
      </div>

      <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index === filteredQuestions.length - 1
            ? 'Finish'
            : getCurrentQuestion.selected === null
              ? 'Select an option'
              : 'Next Question'
        }}
      </button>

      <button @click="startAnotherQuiz">Start Another Quiz</button>
   
    </section>

    <section v-else-if="quizCompleted">
      <h2>You have finished the quiz!</h2>
      <h3>Your score is {{ score }}/{{ filteredQuestions.length }}</h3>
      <button @click="startAnotherQuiz">Start Another Quiz</button>
    </section>
  </main>
</template>