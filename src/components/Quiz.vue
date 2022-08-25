<template>
<div>
  <template v-if="this.question">
    <h1 v-html="this.question"></h1>
   <ul>
    <li v-for="(answer, index) in this.answers" :key="index">
      <input type="radio" name="options" :value="answer" v-model="this.chosen_answer" />

      <label v-html="answer"></label>
    </li>
    </ul>
     <button @click="submitAnswer" class="send" type="button">Send</button>
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
      chosen_answer:undefined
    };
  },
  methods:{
   submitAnswer (){
    if (!this.chosen_answer) {
        alert('Pick one of the options')
    }
   }
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
    this.axios
      .get("https://opentdb.com/api.php?amount=1&category=18")
      .then((response) => {
        (this.question = response.data.results[0].question),
          (this.correctAnswers = response.data.results[0].correct_answer),
          (this.incorrectAnswers = response.data.results[0].incorrect_answers);
        console.log(response.data.results[0]);
      });
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
ul{
    list-style-type: none;
   
}
li{
   
    margin-top: 10px;
    margin-bottom: 10px;
    text-align: center;

}
</style>