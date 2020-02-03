<template>
  <div id="app">

    <!-- HOME PAGE -->
    <div v-show="!started" class="wrapper">
      <div class="start-title animated zoomIn slow">
        <div class="hulla animated slideInLeft slow">
          <h1>HULLA</h1>
        </div>
        <div class="balook animated slideInRight slow">
          <h1>BALOOK</h1>
        </div>
        <div class="quiz">
          <h1>QUIZ</h1>
        </div>
      </div>
      <div class="begin-play">
        <button class="play-button" @click="showGame">PLAY</button>
      </div>
    </div>

    <!-- GAME PLAY SCREEN -->
    <div v-show="started" v-if="!showEndScreen">
      <div class="to-left animated slideInLeft slow">
        <h1>
          KNOW YOUR
          <span class="bold-text">BACKPACK</span>,
        </h1>
      </div>
      <div class="to-right animated slideInRight slow">
        <h1>
          KNOW YOUR
          <span class="bold-text">BRAND.</span>
        </h1>
      </div>
    </div>

    <!-- INSTRUCTIONS AND CONTROLS -->
    <div v-show="started" v-if="!showEndScreen" class="white-area animated slideInUp slow">
      <h3>HOW TO PLAY?</h3>
      <p v-show="started">
        How well do you know your brands? Lets find out!
        <br />
        <br />We are going to put your knowledge to the test. The aim of the game is to guess the brand of the backpack that will appear when the game begins.
        <br />
        <br />Once you have selected a brand click Submit and then Next to get the next question.
        <br />
        <br />You are racing against the clock...Good Luck!
      </p>
      <h3 class="score">SCORE: {{ score }}/ {{ totalScore }}</h3>
      <div v-show="started" class="controls-wrapper">
        <div class="controls">
          <div class="buttons-wrapper">
            <button class="start" v-if="!isActive" @click="start">START</button>
            <button class="stop" v-if="isActive" @click="stop">STOP</button>
            <button class="start" v-if="chosenId !== null" @click="handleSubmit">SUBMIT</button>
            <button class="start" v-if="isSubmitted" @click="nextQuestion">NEXT</button>
          </div>
          <div class="counter" v-if="isActive">{{ seconds }} seconds left!</div>
        </div>
      </div>

      <!-- CORRECT OR WRONG APPEAR -->
      <div v-show="isCorrect && isSubmitted">
        <h1 class="correct-text">CORRECT!</h1>
      </div>
      <div v-if="!isCorrect && isSubmitted">
        <h1 class="wrong-text">WRONG!</h1>
      </div>

      <!-- GAME BOARD AND BAG APPEARS -->
      <div v-show="started" class="animated slideInUp slow bag-showing">
        <Questions :allTheBackpacks="backpacks[index]" :isActive="isActive" :started="started" />
      </div>
      <div v-show="started" class="game">
        <div
          v-for="(brands, index) in brands"
          @click="handleClick(index)"
          :class="[index === logoIndex ? 'selected' : '']"
          :key="index"
        >
          <img :src="brands.image" class="brand-img" alt="brands" />
        </div>
      </div>
    </div>
    <div class="end-screen" v-if="showEndScreen">
      <h1 class="end-results">GAME OVER</h1>
      <h2>YOU SCORED {{ score }}/ {{ totalScore }}</h2>
      <p class="thanks">Thanks for playing!</p>
    </div>
  </div>
</template>

<script>
import Questions from "./components/Questions.vue";

export default {
  name: "app",
  components: {
    Questions
  },
  data() {
    return {
      isActive: false,
      hasWon: false,
      seconds: 30,
      score: 0,
      totalScore: 4,
      clicked: false,
      started: false,
      logoIndex: null,
      isCorrect: false,
      isSubmitted: false,
      chosenId: null,
      showEndScreen: false,
      backpacks: [
        {
          image:
            "https://encrypted-tbn0.gstatic.com/shopping?q=tbn:ANd9GcQ6LxGHlU4ko_0l6VEfJL47ZbQamwmZuNqmd5aXcHBueRHrYsPlb-PzYSYZ9koTAYqoFhq8Zvzp21tSUVl7G0gvj675YY3--1hUecwZCXTFQO7OFOuot2-L&usqp=CAE",
          brand: "herschel"
        },
        {
          image:
            "https://encrypted-tbn1.gstatic.com/shopping?q=tbn:ANd9GcSIYCqM4X_pqvZSbj3gHoSGA1Z15HE3W7NjWVuNMmr4TIBCbO0VLZ7tuuJQNbndVMJgntU77tNk39NzZ5xsyKuR1pxW3b-8Iogw2cQEn-5il9I5GJNtW43O&usqp=CAE",
          brand: "eastpak"
        },
        {
          image:
            "https://encrypted-tbn2.gstatic.com/shopping?q=tbn:ANd9GcS-aSSS5g9KYnbvm4rt5WTxOO-qKdbr1lmbmh48zqsZSym5YFtjsZr_LfF1CZLcNiIGxIJ-q6L1WICe9SH6xMmtYOkAhg1R___Heuzsfw_xV3jKKCruDnln7g&usqp=CAE",
          brand: "kanken"
        },
        {
          image:
            "https://encrypted-tbn0.gstatic.com/shopping?q=tbn:ANd9GcR14lfCPXMMci8B-mNyQ2eglyu6Jn0mYjYpHmWRo7NzZrTGznAcqT0bxVKRLridMH6xTbgt62oLPTSRUZzfvR8zmjT8L_echeU0VR3Pf5vQe40_gS8mwsMh&usqp=CAY",
          brand: "sandqvist"
        }
      ],
      brands: [
        {
          image:
            "https://www.samplesaleguide.co.uk/listing_fotos/large_e7ec6666c4a3.jpg",
          brand: "sandqvist",
          id: 3
        },
        {
          image: "https://cdn.worldvectorlogo.com/logos/herschel.svg",
          brand: "herschel",
          id: 0
        },
        {
          image:
            "https://www.eastpak.com/media/catalog/product/cache/0/image/720x/a4e40ebdc3e371adff845072e1c73f37/placeholder/default/logo.png",
          brand: "eastpak",
          id: 1
        },
        {
          image:
            "https://i.pinimg.com/originals/83/c5/bd/83c5bd4413078920a6ed816bfc0b904e.png",
          brand: "kancken",
          id: 2
        }
      ],
      index: 0
    };
  },
  methods: {
    start() {
      this.isActive = true;
      this.startTimer();
      setInterval(() => {
        if (this.seconds < 0) {
          this.showEndScreen = true;
        }
      }, 50);
    },
    showGame() {
      setInterval(() => {
        this.started = true;
      }, 500);
    },
    startTimer() {
      let timer = setInterval(() => {
        this.seconds -= 1;
      }, 1000);
    },
    stop() {
      this.isActive = false;
    },
    handleClick(index) {
      this.clicked = true;
      this.logoIndex = index;
      this.chosenId = this.brands[index].id;
      if (this.brands[index].id === this.index) {
        this.isCorrect = true;
      }
    },
    nextQuestion() {
      this.index += 1;
      this.isSubmitted = false;
      this.isCorrect = false;
    },
    handleSubmit() {
      this.isSubmitted = true;
      this.chosenId = null;
      if (this.isCorrect) {
        this.score++;
      }
      if (this.index === 3 && this.isSubmitted) {
        this.isActive = false;
        clearInterval(this.timer);
        setTimeout(() => {
          this.stop();
          this.stop;
          this.showEndScreen = true;
        }, 1200);
      }
    }
  }
};
</script>

<style>
@import "./styles/style.css";
</style>

  





