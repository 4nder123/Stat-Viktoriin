<template>
  <section v-if="current < questions.length">
    <div class="error" v-if="error">{{ error }}</div>
    <form v-on:submit.prevent="submitQuestion">
      <h2>{{ questions[current].question }}</h2>
      <label class="answer" :class=" {correct: showAnswer && isAnswerCorrect(index), wrong: showAnswer && !isAnswerCorrect(index)}" v-for="(answer, index) in questions[current].answers" :key="index">
        <input type="radio" name="answer" :value="index" v-model="selectedAnswer" :disabled="showAnswer"/>
        {{ answer }}
      </label>
      <button v-if="showAnswer" type="submit">Edasi</button>
      <button v-else type="submit">Vasta</button>
    </form>
  </section>
  <section v-else>
    <h2 v-if="numOfCorrect===questions.length">Täiuslik!</h2>
    <h2 v-else-if="numOfCorrect>questions.length/2">Hea!</h2>
    <h2 v-else>Proovi uuesti!</h2>
    <div>{{ numOfCorrect + "/" + questions.length }}</div>
    <div class="questions" v-for="(question, qIndex) in questions" :key="qIndex">
      <div>{{ question.question }}</div>
      <label class="answer" :class="index === question.correct? 'correct':'wrong'" v-for="(answer, index) in question.answers" :key="index">
        <input type="radio" :checked="index===selectedAnswers[qIndex]" disabled/>
        {{ answer }}
      </label>
    </div>
    <button @click="reset">Proovi uuesti</button>
  </section>
</template>

<script>
export default {
  name: 'Viktoriin',
  data() {
    return {
      questions: [{question:"Mis on Eesti kõrgeim tipp?", answers:["Kerekunnu mägi","Suur Munamägi","Rohtõsuu mägi"], correct:1},
                {question:"Kes oli Eesti esimene president?", answers:["Konstantin Päts","Toomas Hendrik Ilves","Arnold Rüütel"], correct:0},
                {question:"Mitu saart on Eestil?", answers:["1562","983","2317"], correct:2}],
      selectedAnswers: [],
      selectedAnswer: null,
      showAnswer: false,
      error: null,
      numOfCorrect: 0,
      current: 0,
    }
  },
  methods: {
    submitQuestion() {
      if(this.selectedAnswer === null) { this.error = "Palun vali vastus!"; return;}
      this.error = null;
      if(this.selectedAnswer === this.questions[this.current].correct && !this.showAnswer) { this.numOfCorrect++; }
      if(!this.showAnswer) { this.selectedAnswers.push(this.selectedAnswer); this.showAnswer = true; return; }
      this.showAnswer = false;
      this.selectedAnswer = null;
      this.current++;
    },
    isAnswerCorrect(index){
      return index === this.questions[this.current].correct;
    },
    reset() {
      this.selectedAnswers = [];
      this.numOfCorrect = 0;
      this.current = 0;
    }
  }
}
</script>

<style scoped>
  section {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    max-width: 700px;
    min-height: 500px;
    margin: 30px auto;
    background: #fff;
    padding: 25px;
    box-shadow: 0 4px 24px rgba(0, 0, 0, .05);
  }
  h2 {
    margin: 0px;
  }
  button {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    display: block;
    cursor: pointer;
    margin: 5px auto;
    padding: 10px 30px;
    background: #000;
    color: #fff;
    border: 2px solid #000;
    font-weight: bold;
    transition: 0.2s ease;
  }
  button:hover {
    background: #fff;
    color: #000;
  }
  .error {
    background: #ffe6e6;
    border: 1px solid #f00;
    color: #f00;
    padding: 10px;
  }
  .answer {
    white-space: nowrap;
    padding: 10px;
    margin: 6px auto;
    border: 2px solid #ddd;
    cursor: pointer;
    background: #f9f9f9;
    transition: 0.2s ease;
    margin: 5px;
  }
  .answer:hover:not(.correct):not(.wrong) {
    background: #f1f1f1;
  }
  .answer input {
    margin: 20px 0px;
  }
  .correct {
    cursor: context-menu;
    background-color: #e6ffe6;
    border: 2px solid #0f0;
  }
  .wrong {
    cursor: context-menu;
    background-color: #ffe6e6;
    border: 2px solid #f00;
  }
  .questions {
    text-align: center;
    margin: 10px;
  }
</style>
