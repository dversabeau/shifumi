<template>
  <div id="app">
    <div class="modal" v-if="modal === true">
      <div class="modal-title-container">
        <div class="modal-title">
          <h1>Règles</h1>
        </div>
      </div>
      <div class="modal-image-container">
        <img src="./assets/images/image-rules.svg" alt="">
      </div>
      <div class="close" @click="showModal()">
        <p>+</p>
      </div>
    </div>
    <header class="header">
      <div class="title-container">
        <h1>Pierre <br> Papier <br> Ciseaux</h1>
      </div>
      <div class="score-container">
        <div class="score-title">Score</div>
        <div class="score">{{ score }}</div>
      </div>
    </header>

    <div class="tabletop" v-show="showResult === false">
      <div class="button-choice-container rock">
        <buttonChoice :value="'rock'">
          <img src="./assets/images/icon-rock.svg" alt="Rock">
        </buttonChoice>
      </div>
      <div class="button-choice-container paper">
        <buttonChoice :value="'paper'">
          <img src="./assets/images/icon-paper.svg" alt="Paper">
        </buttonChoice>
      </div>
      <div class="button-choice-container scissors">
        <buttonChoice :value="'scissors'">
          <img src="./assets/images/icon-scissors.svg" alt="Scissors">
        </buttonChoice>
      </div>
    </div>
    <div class="tabletop-results" v-show="showResult === true">
      <div class="show-results-container">
        <div class="player-choice">
            <div :class="{waving: valueResult === 'Gagné'}" class="button-choice-container rock" @click.prevent.stop v-if="valuePlayer === 'rock'" >
              <buttonChoice>
                <img src="./assets/images/icon-rock.svg" alt="Rock">
              </buttonChoice>
            </div>
           <div :class="{waving: valueResult === 'Gagné'}" class="button-choice-container paper" @click.prevent.stop v-if="valuePlayer === 'paper'">
             <buttonChoice>
               <img src="./assets/images/icon-paper.svg" alt="Paper">
             </buttonChoice>
           </div>
            <div :class="{waving: valueResult === 'Gagné'}" class="button-choice-container scissors" @click.prevent.stop v-if="valuePlayer === 'scissors'">
              <buttonChoice>
                <img src="./assets/images/icon-scissors.svg" alt="Scissors">
              </buttonChoice>
            </div>
          <div class="player-choice-text">
            <p>Votre choix</p>
          </div>
        </div>
        <div class="house-choice">
          <div class="dark-background"></div>
          <transition name="house-choice">
            <div :class="{waving: valueResult === 'Perdu'}" class="button-choice-container rock" @click.prevent.stop v-if="valueHouse === 'rock' && showResult === true">
              <buttonChoice>
                <img src="./assets/images/icon-rock.svg" alt="Rock">
              </buttonChoice>
            </div>
          </transition>
          <transition name="house-choice">
            <div :class="{waving: valueResult === 'Perdu'}" class="button-choice-container paper" @click.prevent.stop v-if="valueHouse === 'paper'  && showResult === true">
              <buttonChoice :value="'paper'">
                <img src="./assets/images/icon-paper.svg" alt="Paper">
              </buttonChoice>
            </div>
          </transition>
          <transition name="house-choice">
            <div :class="{waving: valueResult === 'Perdu'}" class="button-choice-container scissors" @click.prevent.stop v-if="valueHouse === 'scissors' && showResult === true">
              <buttonChoice :value="'scissors'">
                <img src="./assets/images/icon-scissors.svg" alt="Scissors">
              </buttonChoice>
            </div>
          </transition>

          <div class="house-choice-text">
            <p>Choix ordinateur</p>
          </div>
        </div>
      </div>
      <transition name="results-fade">
        <div class="results-container" v-show="showResult">
            <div class="results-text">{{ valueResult }}</div>
            <div class="results-button" @click="showResult = false">
              <p>Rejouer</p>
            </div>
        </div>
      </transition>
    </div>

    <div class="rules-button-container">
      <div class="rules-button" @click="showModal()" >
        <p>
          Règles
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import buttonChoice from './components/Button.vue'

export default {
  name: 'App',
  components: {
    buttonChoice,
  },
  data: function () {
    return {
      valuePlayer: null,
      valueHouse: null,
      valueResult: null,
      score: 0,
      modal: false,
      showResult: false
    }
  },
  methods: {
    setHouseChoice: function () {
      let index = Math.floor(Math.random() * Math.floor(3))
      switch (index) {
        case 0: this.valueHouse = 'rock'; return;
        case 1: this.valueHouse = 'paper'; return;
        case 2: this.valueHouse = 'scissors'; return;
      }
    },
    setResult: function () {
      if (this.valueHouse === this.valuePlayer){
        this.valueResult = 'Égalité'
      } else if (this.valueHouse === 'rock' && this.valuePlayer === 'scissors'){
        this.valueResult = 'Perdu'
        this.decrementsScore()
      } else if (this.valueHouse === 'paper' && this.valuePlayer === 'rock'){
        this.valueResult = 'Perdu'
        this.decrementsScore()
      } else if (this.valueHouse === 'scissors' && this.valuePlayer === 'paper'){
        this.valueResult = 'Perdu'
        this.decrementsScore()
      } else {
        this.valueResult = 'Gagné'
        this.incrementsScore()
      }
    },
    incrementsScore: function () {
      this.score += 1;
    },
    decrementsScore: function () {
      if (this.score > 0){
        this.score -= 1;
      } else {
        this.score = 0;
      }
    },
    showModal: function () {
      if (this.modal === true){
        this.modal = false
      } else {
        this.modal = true
      }
    }
  },

  created() {
    this.$on('PlayerValue', function (event) {
      this.valuePlayer = event
      this.setHouseChoice()
      this.setResult()
      this.showResult = true
    })
  },

}
</script>

<style>
  *{
    box-sizing: border-box;
  }
  html, body{
    margin: 0;
    padding: 0;
    font-family: 'Barlow Semi Condensed', sans-serif;
  }
  #app {
    background: radial-gradient(hsl(214, 47%, 23%), hsl(237, 49%, 15%));
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    padding: 30px 30px 60px 30px;
    min-height: 100vh;
  }

  .header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    border: 3px solid hsl(217, 16%, 45%);
    border-radius: 8px;
    width: 100%;
    max-width: 600px;
    padding: 15px;
  }

  .title-container>h1{
    color: white;
    font-weight: 600;
    line-height: .8em;
    text-transform: uppercase;
    font-size: 24px;
    margin: 0;
  }

  .score-container{
    height: 75px;
    width: 80px;
    background: white;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .score-title{
    font-size: 12px;
    font-weight: 700;
    color: hsl(229, 64%, 46%);
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .score{
    font-weight: 700;
    color: hsl(229, 25%, 31%);
    font-size: 40px;
  }

  .tabletop{
    display: grid;
    gap:  30px 40px;
    align-items: center;
    justify-items: center;
    background: url("assets/images/bg-triangle.svg") no-repeat bottom center;
    background-size: 90%;
  }

  .button-choice-container{
    width: 100px;
    height: 100px;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: 0.3s ease-in-out;
    box-shadow: 0 -4px inset rgba(0,0,0,0.25);
    position: relative;
  }

  .dark-background{
    position: absolute;
    top: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background: rgba(0,0,0,0.25);
  }


  .rock{
    background: linear-gradient(to top, hsl(349, 71%, 52%), hsl(349, 70%, 56%));
  }
  .paper{
    background: linear-gradient(to top, hsl(230, 89%, 62%), hsl(230, 89%, 65%));
  }
  .scissors{
    background: linear-gradient(to top, hsl(39, 89%, 49%), hsl(40, 84%, 53%));
    grid-column: span 2 / auto;
  }

  .rules-button-container{
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .rules-button{
    padding: 12px 50px;
    border: solid rgba(255,255,255,0.75) 1px;
    border-radius: 8px;
    transition: 0.3s ease-in-out;
    cursor: pointer;
  }

  .rules-button:hover{
    background: white;
  }

  .rules-button:hover p{
    color: hsl(229, 25%, 31%);
  }

  .rules-button>p{
    margin: 0;
    padding: 0;
    font-size: 20px;
    font-weight: 600;
    color: white;
    text-transform: uppercase;
    letter-spacing: 2px;
  }

  .modal{
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    background: white;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    padding: 30px;
  }

  .modal-title-container{
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal-title{
    color: hsl(229, 25%, 31%);
    text-transform: uppercase;
  }


  .close{
    display: flex;
    width: 100%;
    justify-content: center;
    align-items: center;
    height: 75px;
  }

  .close>p{
    font-size: 75px;
    color: hsl(217, 16%, 45%);
    opacity: 0.35;
    margin: 0;
    padding: 0;
    font-weight: normal;
    transform: rotate(45deg);
    cursor: pointer;
    line-height: 0;
    transform-origin: center;
  }

  .tabletop-results{
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
  }

  .show-results-container{
    display: flex;
    justify-content: space-between;
    width: 100%;
    max-width: 350px;
    margin-bottom: 50px;
  }


  .player-choice, .house-choice{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .house-choice{
    position: relative;
  }

  .player-choice-text, .house-choice-text{
    text-transform: uppercase;
    color: white;
    letter-spacing: 2px;
  }

  .results-container{
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .results-text{
    color: white;
    text-transform: uppercase;
    font-size: 40px;
    padding-bottom: 15px;
  }

  .results-button{
    background: white;
    border-radius: 10px;
    text-transform: uppercase;
    padding: 15px 35px;
    font-size: 16px;
    letter-spacing: 2px;
    cursor: pointer;
  }
  .results-button>p{
    padding: 0;
    margin: 0;
  }

  .player-choice>.button-choice-container,.house-choice>.button-choice-container{
    pointer-events: none;
  }

  .waving{
    animation: waving 1s .6s infinite;
  }

  @keyframes waving {
    from{
      transform: scale(1);
    }
    50%{
      transform: scale(1.1);
    }
    to{
      transform: scale(1);
    }
  }

  .house-choice-enter-active{
    transition: opacity 0.5s 0.5s;
  }

  .house-choice-leave-active{
    transition: 0s;
  }

  .house-choice-enter, .house-choice-leave-to{
    opacity: 0;
  }

  .results-fade-enter-active{
    transition: opacity 0.5s 0.8s;
  }

  .results-fade-enter, .results-fade-leave-to{
    opacity: 0;
  }
  .results-fade-leave-active{
    transition: 0s;
  }
</style>
