<template>
  <div id="app">
    <Header 
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
        <QuestionBox  
          v-if="questions.length"
          :currentQuestion="questions[index]"
          :next="next"
          :increment="increment"
        />
        </b-col>
      </b-row>
    </b-container>
    <Result 
      :visible="index == 10"
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
  </div>
</template>

<script>
import Header from './components/Header'
import QuestionBox from './components/QuestionBox'
import Result from './components/Result'
export default {
  name: 'App',
  components: {
    Header,
    QuestionBox,
    Result
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect : 0,
      numTotal : 0
    };
  },
  methods : {
    next : function (){
      this.index++;
    },
    increment(isCorrect) {
      if(isCorrect){
          this.numCorrect++;
      }
      this.numTotal++;
    }
  },
  mounted : function() {
    fetch('https://opentdb.com/api.php?amount=10&category=31&difficulty=easy&type=multiple', {
      method: 'get'
    })
      .then((response) => {
        return response.json()
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
