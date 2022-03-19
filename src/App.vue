<template>
  <div class="flex flex-col justify-center bg-indigo-600 h-screen">
    <div class="position absolute top-0 flex flex-row ml-3">
      <div
        :class="[
          'px-4 py-2 my-2 mx-1 text-[13px]',
          lang === 'es' ? 'bg-slate-800 text-white' : 'bg-indigo-600',
          'outline outline-1 outline-slate-800 rounded-md',
          'hover:bg-slate-800 hover:text-slate-200',
        ]"
        @keyup="() => {}"
        @click="setSpanish()"
        style="cursor: pointer"
      >
        Español
      </div>
      <div
        :class="[
          'px-4 py-2 my-2 mx-1 text-[13px]',
          lang === 'en' ? 'bg-slate-800 text-white' : 'bg-indigo-600',
          'outline outline-1 outline-slate-800 rounded-md',
          'hover:bg-slate-800 hover:text-slate-200',
        ]"
        @keyup="() => {}"
        @click="setEnglish()"
        style="cursor: pointer"
      >
        English
      </div>
    </div>
    <div class="flex flex-row justify-center bg-indigo-600">
      <QuizForm
        :questionData="questionData"
        @nextQuestion="nextQuestion"
        :points="points"
        @tryAgain="tryAgain"
        :lang="lang"
        :title="title"
      />
    </div>
  </div>
</template>

<script>
import QuizForm from './components/QuizForm.vue';

const axios = require('axios');

export default {
  name: 'App',
  components: { QuizForm },
  data() {
    return {
      baseUrl: 'https://restcountries.com/v3.1/all',
      points: 0,
      lang: 'en',
      title: {
        en: 'COUNTRY QUIZ',
        es: 'QUIZ DE PAISES',
      },
      questionData: {
        image: '',
        question: '....................loading',
        options: [
          { text: '........', checked: false },
          { text: '........', checked: false },
          { text: '........', checked: false },
          { text: '........', checked: false },
        ],
        correctOption: 'Syria',
      },
      countries: [],
    };
  },
  mounted() {
    this.getCountries();
    document.title = 'DevChallenges | Country Quiz';
  },
  methods: {
    setSpanish() {
      this.lang = 'es';
    },
    setEnglish() {
      this.lang = 'en';
    },
    nextQuestion(valid) {
      if (valid) {
        this.points += 1;
        this.randomQuestion();
      }
    },
    tryAgain() {
      this.points = 0;
      this.randomQuestion();
    },
    randomQuestion() {
      const elec = Math.round(Math.random() * 3);
      if (elec === 1) {
        this.getQuestionAboutCountryFlag();
      } else if (elec === 2) {
        this.getQuestionAboutCountryCapital();
      } else if (elec === 3) {
        this.getQuestionAboutCountrySize();
      }
    },
    async getCountries() {
      const res = await axios.get(this.baseUrl);
      if (res) {
        this.countries = res.data;
        console.log(this.countries);
        this.randomQuestion();
      }
    },
    getQuestionAboutCountryCapital() {
      const ctrs = [
        this.countries[Math.round(Math.random() * 250)],
        this.countries[Math.round(Math.random() * 250)],
        this.countries[Math.round(Math.random() * 250)],
      ];

      const options = [
        {
          text: ctrs[0].name.common,
          checked: false,
          translatedText: ctrs[0].translations.spa.common,
        },
        {
          text: ctrs[1].name.common,
          checked: false,
          translatedText: ctrs[1].translations.spa.common,
        },
        {
          text: ctrs[2].name.common,
          checked: false,
          translatedText: ctrs[2].translations.spa.common,
        },
      ];

      const country = this.countries[Math.round(Math.random() * 250)];
      const image = false;
      const correctOption = country.name.common;
      options.push({
        text: correctOption,
        checked: false,
        translatedText: country.translations.spa.common,
      });
      options.sort(() => Math.random() - 0.5);
      const question = `${country.capital[0]} is the capital of?`;
      const translatedQuestion = `${country.capital[0]} es la capital de?`;

      const data = {
        image,
        question,
        translatedQuestion,
        options,
        correctOption,
      };
      this.questionData = data;
    },
    getQuestionAboutCountryFlag() {
      const ctrs = [
        this.countries[Math.round(Math.random() * 250)],
        this.countries[Math.round(Math.random() * 250)],
        this.countries[Math.round(Math.random() * 250)],
      ];

      const options = [
        {
          text: ctrs[0].name.common,
          checked: false,
          translatedText: ctrs[0].translations.spa.common,
        },
        {
          text: ctrs[1].name.common,
          checked: false,
          translatedText: ctrs[1].translations.spa.common,
        },
        {
          text: ctrs[2].name.common,
          checked: false,
          translatedText: ctrs[2].translations.spa.common,
        },
      ];

      const country = this.countries[Math.round(Math.random() * 250)];
      const image = country.flags.png;
      const correctOption = country.name.common;
      options.push({
        text: correctOption,
        checked: false,
        translatedText: country.translations.spa.common,
      });
      options.sort(() => Math.random() - 0.5);
      const question = 'Which country does this flag belong to?';
      const translatedQuestion = 'A qué país pertenece esta bandera?';

      const data = {
        image,
        question,
        translatedQuestion,
        options,
        correctOption,
      };
      this.questionData = data;
    },
    getQuestionAboutCountrySize() {
      const ctrs = [
        this.countries[Math.round(Math.random() * 250)],
        this.countries[Math.round(Math.random() * 250)],
        this.countries[Math.round(Math.random() * 250)],
        this.countries[Math.round(Math.random() * 250)],
      ];

      let largestCountry = {
        area: 0,
        name: '',
        translatedName: '',
      };
      ctrs.forEach((item) => {
        if (item.area > largestCountry.area) {
          largestCountry = {
            area: item.area,
            name: item.name.common,
            translatedName: item.translations.spa.common,
          };
        }
      });

      const options = [
        {
          text: ctrs[0].name.common,
          translatedText: ctrs[0].translations.spa.common,
          checked: false,
        },
        {
          text: ctrs[1].name.common,
          translatedText: ctrs[1].translations.spa.common,
          checked: false,
        },
        {
          text: ctrs[2].name.common,
          translatedText: ctrs[2].translations.spa.common,
          checked: false,
        },
        {
          text: ctrs[3].name.common,
          translatedText: ctrs[3].translations.spa.common,
          checked: false,
        },
      ];

      const image = false;
      const correctOption = largestCountry.name;
      options.sort(() => Math.random() - 0.5);
      const question = 'Which country is the largest one?';
      const translatedQuestion = 'Qué país es el mas grande?';

      const data = {
        image,
        question,
        options,
        translatedQuestion,
        correctOption,
      };
      this.questionData = data;
    },
  },
};
</script>

<style>
* {
  padding: 0px;
  margin: 0px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
