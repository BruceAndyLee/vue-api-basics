<template>
  <div class="questionbox">
    <div class="questionbox__header">
      {{ index + 1 }}
      {{ currentQuestion.question | fixApostrophe | fixQuotation }}
    </div>
    <div class="questionbox__divider"></div>
    <div>
      <ul class="questionbox__options">
        <li
          v-for="(option, ind) in shuffledOptions"
          :key="option"
          class="questionbox__option"
          :class="{
            selected: isSelected(ind) && !isSubmitted,
            correct: ind === correctIndex,
            incorrect: ind !== correctIndex && selectedIndex === ind
          }"
          @click="selectOption(option, ind)"
        >
          {{ option | fixApostrophe | fixQuotation }}
        </li>
      </ul>
    </div>
    <div class="questionbox__buttons">
      <button 
        class="nicebutton"
        @click="submitOption()"
        :disabled="!selectedIndex && isSubmitted"
      >
        Submit
      </button>
      <button class="nicebutton" @click="next">Next</button>
    </div>
  </div>
</template>

<script>

import _ from 'lodash'

export default {
  props: {
    index: Number,
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledOptions: null,
      isSubmitted: false
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleOptions();
        this.correctIndex = this.shuffledOptions.indexOf(this.currentQuestion.correct_answer);
        this.isSubmitted = false;
      }
    },
  },
  methods: {
    selectOption(option, ind) {
      if (this.isSubmitted) {
        return
      }
      this.selectedIndex = ind;
      if (this.shuffledOptions[ind] === this.currentQuestion.correct_answer) {
        console.log("hell yes ", option);
      }
    },
    submitOption() {
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }

      this.isSubmitted = true;

      this.increment(isCorrect);
    },
    isSelected(ind) {
      return ind === this.selectedIndex && !this.isSubmitted;
    },
    shuffleOptions() {
      let options = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledOptions = _.shuffle(options);
      return options;
    },
  },
  filters: {
    fixQuotation: function (value) {
      return value.replaceAll("&quot;", '"');
    },
    fixApostrophe: function (value) {
      return value.replaceAll("&#039;", "'");
    },
  },
  computed: {
    options() {
      let options = [...this.currentQuestion.incorrect_answers];
      options.push(this.currentQuestion.correct_answer);
      return options;
    },
  },
  updated: function () {},
};
</script>


<style>
.questionbox {
  padding: 30px;
  background-color: lightblue;
  width: 60%;
  margin-left: 20%;
  margin-right: 20%;
}

.questionbox__divider {
  width: 100%;
  height: 1px;
  background-color: grey;
}

.questionbox__header {
  padding-top: 10px;
  padding-bottom: 10px;
}

.questionbox__options {
  padding-top: 10px;
  padding-bottom: 10px;
  list-style-type: none;
}

.questionbox__option {
  padding: 15px;
  background-color: white;
  border-top: 1px solid rgba(24, 128, 128, 0.534);
  border-left: 1px solid rgba(24, 128, 128, 0.534);
  border-right: 1px solid rgba(24, 128, 128, 0.534);
}

.questionbox__option:last-child {
  border-bottom: 1px solid rgba(24, 128, 128, 0.534);
}

.questionbox__option:hover {
  box-shadow: inset 0 0 10px 5px rgb(219, 219, 219);
  cursor: pointer;

}

.selected {
  background-color: lightgrey;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: rgb(250, 145, 145);
}

.questionbox__buttons {
  padding-top: 30px;
}

.nicebutton {
  font-size: 17px;
  outline: none;
  border: none;
  padding: 10px;
  margin: 5px;
  border-radius: 2px;
}

.nicebutton:hover {
  background-color: lightseagreen;
}
</style>