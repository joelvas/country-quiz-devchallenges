<template>
  <div class="flex flex-row justify-center bg-indigo-600 items-center h-screen">
    <QuizForm
      :questionData="questionData"
      @setResult="handleResult"
      :points="points"
      @tryAgain="tryAgain"
    />
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
      questionData: {
        image: '',
        question: '-----------------------------------?',
        options: [
          { text: '--------', checked: false },
          { text: '--------', checked: false },
          { text: '--------', checked: false },
          { text: '--------', checked: false },
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
    handleResult(valid) {
      if (valid) {
        this.points += 1;
        this.randomQuestion();
      }
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
        this.randomQuestion();
      }
    },
    tryAgain() {
      this.points = 0;
      this.randomQuestion();
    },
    getQuestionAboutCountryCapital() {
      const options = [
        { text: this.countries[Math.round(Math.random() * 250)].name.common, checked: false },
        { text: this.countries[Math.round(Math.random() * 250)].name.common, checked: false },
        { text: this.countries[Math.round(Math.random() * 250)].name.common, checked: false },
      ];

      const country = this.countries[Math.round(Math.random() * 250)];
      const image = false;
      const correctOption = country.name.common;
      options.push({ text: correctOption, checked: false });
      options.sort(() => Math.random() - 0.5);
      const question = `${country.capital[0]} is the capital of?`;

      const data = {
        image,
        question,
        options,
        correctOption,
      };
      this.questionData = data;
    },
    getQuestionAboutCountryFlag() {
      const options = [
        { text: this.countries[Math.round(Math.random() * 250)].name.common, checked: false },
        { text: this.countries[Math.round(Math.random() * 250)].name.common, checked: false },
        { text: this.countries[Math.round(Math.random() * 250)].name.common, checked: false },
      ];

      const country = this.countries[Math.round(Math.random() * 250)];
      const image = country.flags.png;
      const correctOption = country.name.common;
      options.push({ text: correctOption, checked: false });
      options.sort(() => Math.random() - 0.5);
      const question = 'Which country does this flag belong to?';

      const data = {
        image,
        question,
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
        country: '',
      };
      ctrs.forEach((item) => {
        if (item.area > largestCountry.area) {
          largestCountry = {
            area: item.area,
            country: item.name.common,
          };
        }
      });

      const options = [
        { text: ctrs[0].name.common, checked: false },
        { text: ctrs[1].name.common, checked: false },
        { text: ctrs[2].name.common, checked: false },
        { text: ctrs[3].name.common, checked: false },
      ];

      const image = false;
      const correctOption = largestCountry.country;
      options.sort(() => Math.random() - 0.5);
      const question = 'Which country is the largest?';

      const data = {
        image,
        question,
        options,
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
