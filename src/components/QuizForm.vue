<template>
  <div class="w-5/6 md:w-1/2 lg:w-2/6">
    <div class="text-[20px] font-bold pb-2 text-white">COUNTRY QUIZ</div>
    <div class="bg-white rounded-md flex flex-col items-center justify-center h-96" v-if="results">
      <div class="text-[40px] font-bold text-slate-800">Results</div>
      <div class="flex flex-row items-center">
        You got
        <div class="px-2 text-[24px] font-bold text-green-600">{{ points }}</div>
        correct answers
      </div>
      <div class="mt-12">
        <button
          class="outline outline-2 outline-slate-800 rounded-md py-2 px-8 text-[13px] font-bold"
          @click="tryAgain"
        >
          Try Again
        </button>
      </div>
    </div>
    <div class="bg-white rounded-md text-slate-700 px-5 pt-11 pb-7" v-else>
      <div v-if="typeof question.image === 'string'">
        <img :src="question.image" class="w-20" alt="flag" />
      </div>
      <div class="font-extrabold mt-4">{{ question.question }}</div>
      <div class="pt-4">
        <div
          style="cursor: pointer"
          @keyup="() => {}"
          @click="showCorrectAnswer(item, i)"
          v-for="(item, i) in question.options"
          :key="i"
          :class="[
            'flex flex-row items-center',
            'outline outline-1 outline-slate-500',
            'rounded-md',
            'p-2 mb-5',
            answered && item.checked ? 'bg-red-600' : 'bg-white',
            !answered ? 'hover:bg-yellow-400 hover:text-white group hover:outline-white' : '',
            answered && item.text === question.correctOption
              ? 'bg-green-600 text-white'
              : 'bg-white',
          ]"
        >
          <div
            :class="[
              (answered && item.text === question.correctOption) || (answered && item.checked)
                ? 'text-white'
                : 'text-slate-500',
              'group-hover:text-white',
              'mr-5',
            ]"
          >
            {{ letters[i] }}
          </div>
          <div
            :class="[
              (answered && item.text === question.correctOption) || (answered && item.checked)
                ? 'text-white'
                : 'text-slate-500',
              'group-hover:text-white',
            ]"
          >
            {{ item.text }}
          </div>
        </div>
      </div>
      <div class="flex flex-row justify-end" v-if="answered">
        <button class="bg-yellow-500 px-6 py-2 rounded-md text-white" @click="handleResult">
          Next
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    questionData: {
      type: Object,
      default() {
        return {};
      },
    },
    points: {
      type: Number,
      default() {
        return 0;
      },
    },
  },
  data() {
    return {
      letters: ['A', 'B', 'C', 'D'],
      question: {},
      answered: false,
      valid: false,
      results: false,
    };
  },
  watch: {
    questionData() {
      this.setQuestion();
    },
  },
  mounted() {
    this.setQuestion();
  },
  methods: {
    setQuestion() {
      this.question = this.questionData;
    },
    showCorrectAnswer(item, i) {
      this.answered = true;
      if (item.text === this.question.correctOption) {
        this.valid = true;
      } else if (item.text !== this.question.correctOption) {
        this.valid = false;
        this.question.options[i].checked = true;
      }
    },
    handleResult() {
      if (this.valid) {
        this.$emit('setResult', this.valid);
        this.answered = false;
        this.valid = false;
      } else {
        this.results = true;
      }
    },
    tryAgain() {
      this.$emit('tryAgain');
      this.results = false;
      this.answered = false;
      this.valid = false;
    },
  },
};
</script>

<style></style>
