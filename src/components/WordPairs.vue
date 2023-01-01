<template>

  <div class="card">
    <div class="card-content column">
      <div v-if="currentPair">
        <img class="card-image" :src="currentPair.image"/>
        <p class="card-text" v-if="language === 'english'">{{ currentPair.english }}</p>
        <p class="card-text" v-else>{{ currentPair.serbian }}</p>
        <input class="card-input " v-model="userAnswer" @keyup.enter="checkAnswer" required/>
        <button class="card-button" @click="checkAnswer">Let's see</button>
        <div class="counter-container">
          <div class="counter">
            <p>
              <i class="fa-light fa-thumbs-up"></i> {{ correctAnswers }}/{{ totalQuestions }}
            </p>
          </div>
          <div class="counter">
            <p>
              <i class="fa-light fa-thumbs-up"></i> {{ incorrectAnswers }}/{{ totalQuestions }}
            </p>
          </div>
        </div>
      </div>
      <div v-else>
        <p>No more pairs left. Resetting the game...</p>
      </div>
    </div>
  </div>
</template>


<style scoped>

.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 300px;
  height: 400px;
  padding: 20px;
  background-color: white;
  border-radius: 10px;
}

.card-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: 10px 10px 0 0;
}

.card-text {
  margin: 10px 0;
  font-size: 18px;
  font-weight: bold;
  text-align: center;
  color: #222222;
}

.card-input {
  width: 100%;
  height: 40px;
  padding: 10px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  margin-bottom: 10px;
  box-shadow: 2px 2px 5px orangered;
}

.card-button {
  margin-top: 10px;
  width: 100%;
  height: 40px;
  background-color: orangered;
  color: white;
  font-size: 16px;
  font-weight: bold;
  border: none;
  border-radius: 5px;
  cursor: pointer;

}

.counter-container {
  display: flex;
}

.counter {
  flex: 1;
  text-align: center;
  font-size: 18px;
  font-weight: bold;
  color: orangered;
}

button {
  margin-top: 16px;
  padding: 8px 16px;
  font-size: 18px;
  font-weight: bold;
  border: none;
  border-radius: 4px;
  background-color: #ff4500;
  color: #fff;
  cursor: pointer;
}

@media (min-width: 1280px) and (max-width: 1920px ) {
  .card {
    width: 550px;
    height: 500px;
  }

  .card-image {
    height: 300px;
  }
}
</style>

<script>
import axios from 'axios';
import config from "@/config";

axios.defaults.baseURL = config.baseUrl;

export default {
  data() {
    return {
      wordPairs: [],
      currentPair: null,
      language: 'english',
      userAnswer: '',
      correctAnswers: 0, // number of correct answers
      incorrectAnswers: 0, // number of incorrect answers
      totalQuestions: 0, // total number of questions

    };
  },
  created() {
    this.fetchWordPairs();
  },
  methods: {
    fetchWordPairs() {
      axios.get('/words').then(response => {
        this.wordPairs = response.data["data"];
        this.showNextWordPair();
      });
    },
    showNextWordPair() {
      const index = Math.floor(Math.random() * this.wordPairs.length);
      this.currentPair = this.wordPairs[index];
      console.log(this.currentPair);
      this.language = Math.random() < 0.5 ? 'english' : 'serbian';
      this.userAnswer = '';
    },
    checkAnswer() {
      if (this.language === 'english') {
        if (this.userAnswer.toLowerCase() === this.currentPair.serbian.toLowerCase()) {
          this.correctAnswers++;
          this.totalQuestions++;
          this.wordPairs.splice(this.wordPairs.indexOf(this.currentPair), 1);
        } else {
          this.incorrectAnswers++;
          this.totalQuestions++;
        }
      } else {
        if (this.userAnswer.toLowerCase() === this.currentPair.english.toLowerCase()) {
          this.correctAnswers++;
          this.totalQuestions++;
          this.wordPairs.splice(this.wordPairs.indexOf(this.currentPair), 1);
        } else {
          this.incorrectAnswers++;
          this.totalQuestions++;
        }
      }
      // show the next word pair or reset the game if there are no more pairs left
      if (this.wordPairs.length > 0) {
        this.showNextWordPair();
      } else {
        this.currentPair = null;
        axios.post('/words', {
          good_answers: this.correctAnswers,
          wrong_answers: this.incorrectAnswers,
          user_id: 100
        });
        this.userAnswer = '';
        this.correctAnswers = 0;// number of correct answers
        this.incorrectAnswers = 0; // number of incorrect answers
        this.totalQuestions = 0;
      }
    },
  },
};
</script>

