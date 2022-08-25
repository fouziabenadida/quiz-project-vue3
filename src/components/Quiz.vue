<template>
  <div>
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>
      <ul>
        <li v-for="(answer, index) in this.answers" :key="index">
          <input
            type="radio"
            name="options"
            :value="answer"
            v-model="this.chosenAnswer"
          />

          <label v-html="answer"></label>
        </li>
      </ul>
      <button
        v-if="!this.answerSubmitted"
        @click="submitAnswer"
        class="send"
        type="button"
      >
        Send
      </button>

      <section v-if="this.answerSubmitted" class="result">
        <h4 vh  v-if="this.chosenAnswer === this.correctAnswers" v-html="  '&#9989; Congratulations, the answer' +  this.correctAnswers + 
          'is correct. '" >
        
        </h4>
        <h4 v-else v-html=" ' &#10060; I`m sorry, you picked the wrong answer. The right one is:' +
            this.correctAnswers + '.'
          ">
        
        </h4>

        <button @click="this. getNewQuetion()" class="send" type="button">Next Question</button>
      </section>
    </template>
  </div>
</template>

<script>
export default {
  name: "quiz-vue",
  data() {
    return {
      question: undefined,
      correctAnswers: undefined,
      incorrectAnswers: undefined,
      chosenAanswer: undefined,
      answerSubmitted: false,
    };
  },
  methods: {
    submitAnswer() {
      if (!this.chosenAnswer) {
        alert("Pick one of the options");
      } else {
        this.answerSubmitted = true;
        if (this.chosenAnswer == this.correctAnswers) {
          console.log("Bravo!! ");
        } else {
          console.log("you got it wrong");
        }
      }
    },
    getNewQuetion  () {
        this.question= undefined,
        this.answerSubmitted= false,
        this.chosenAanswer= undefined

      this.axios
        .get("https://opentdb.com/api.php?amount=1&category=18")
        .then((response) => {
          (this.question = response.data.results[0].question),
            (this.correctAnswers = response.data.results[0].correct_answer),
            (this.incorrectAnswers =
              response.data.results[0].incorrect_answers);
          console.log(response.data.results[0]);
        });
    },
  },

  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
      answers.splice(
        Math.round(Math.random() * answers.length),
        0,
        this.correctAnswers
      );
      return answers;
    },
  },

  created() {
    this.getNewQuetion();
  },
};
</script>




<style scoped>
input [type="radio"] {
  margin: 12px 4px;
}

.send {
  margin-top: 12px;
  height: 40px;
  width: 200px;
  padding: 0 30px;
  color: #fff;
  background-color: #1867c0;
  cursor: pointer;
}
ul {
  list-style-type: none;
}
li {
  margin-top: 10px;
  margin-bottom: 10px;
  text-align: center;
}
</style>