<template>
  <div id="app">
    <div class="container">
      <h1 class="h1-title">Simon Says</h1>
      <div class="simon">
        <div class="simon__item simon__game-place">
          <div class="simon__game" @click="theGame">
            <button 
            class="game_item simon__yellow"
            :style="addOpacity(yellowPush)"
            @click="yellowClick"
            :disabled="isGameDisabled"
            data-number="0"
            ></button>
            <button 
            class="game_item simon__blue"
            :style="addOpacity(bluePush)"
            @click="blueClick"
            :disabled="isGameDisabled"
            data-number="1"
            ></button>
            <button 
            class="game_item simon__green"
            :style="addOpacity(greenPush)"
            @click="greenClick"
            :disabled="isGameDisabled"
            data-number="2"
            ></button>
            <button 
            class="game_item simon__red"
            :style="addOpacity(redPush)"
            @click="redClick"
            :disabled="isGameDisabled"
            data-number="3"
            ></button>
          </div>
        </div>
        <div class="simon__item simon__interface">
          <h2 class="h2-title">Round: {{round}}</h2>
          <button 
          @click="startGame" 
          class="simon__start"
          :disabled="isButtonDisabled">Start</button>
          <p class="simon__lose" :class="lose ? 'valid' : ''">You Lose :(</p>
          <div class="simon__level">
            <h3 class="h3-title">Difficulty:</h3>
            <div class="simon__radio">
              <label>
                  <input 
                  type="radio" 
                  class="radio"  
                  name="difficulty"
                  v-model="timeInterval"
                  :disabled="isButtonDisabled"
                  value="1500">
                  <span class="radio-label">Easy</span>
              </label>
          </div>
          <div class="simon__radio">
              <label>
                  <input 
                  type="radio" 
                  class="radio" 
                  name="difficulty"
                  v-model="timeInterval"
                  :disabled="isButtonDisabled"
                  value="1000">
                  <span class="radio-label">Normal</span>
              </label>
          </div>  
          <div class="simon__radio">
              <label class="radio__container">
                  <input 
                  type="radio" 
                  class="radio" 
                  name="difficulty"
                  v-model="timeInterval"
                  :disabled="isButtonDisabled"
                  value="400">
                  <span class="radio-label">Hard</span>
              </label>
          </div> 
          <h4 class="h4-title">Record: {{record}}</h4>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>



export default {
  name: 'App',
  data() {
    return {
      round: 1,
      record: 0,
      sequence: [],
      timeInterval: 1000,
      timeOfFlash: 300,
      itemNumber: 0,
      isButtonDisabled: false,
      isGameDisabled: true,
      yellowPush: false,
      bluePush: false,
      greenPush: false,
      redPush: false,
      lose: false,
      
    }
  },
  methods: {
    startGame() {
      const btn = Math.floor(Math.random() * 4);
      this.sequence.push(btn);

      this.lose = false;
      this.itemNumber = 0;
      
      this.isButtonDisabled = true;
      setTimeout(() => {
        this.isGameDisabled = false;
      }, this.sequence.length*this.timeInterval+this.timeOfFlash)

      this.sequence.forEach((el, id) => {
        setTimeout(() => {
          switch(el) {
            case 0: this.yellowPush = !this.yellowPush;
              this.playAudio(1);            
              setTimeout(() => {
                this.yellowPush = !this.yellowPush;
              }, this.timeOfFlash);
              break;
            case 1: this.bluePush = !this.bluePush;
              this.playAudio(2);            
              setTimeout(() => {
                this.bluePush = !this.bluePush;
              }, this.timeOfFlash);
              break;
            case 2: this.greenPush = !this.greenPush;
              this.playAudio(3);            
              setTimeout(() => {
                this.greenPush = !this.greenPush;
              }, this.timeOfFlash);
              break;
            case 3: this.redPush = !this.redPush;
              this.playAudio(4);            
              setTimeout(() => {
                this.redPush = !this.redPush;
              }, this.timeOfFlash);
              break;
            }
        }, (id+1)*this.timeInterval)
      });
    },
    theGame(e) {
      if(!this.sequence.length)
        return;

      const btnNumber = e.target.getAttribute('data-number');

      if(btnNumber != this.sequence[this.itemNumber]) {
        this.lose = true;
        this.sequence = [];
        this.round = 1;
        this.isGameDisabled = true;
        this.isButtonDisabled = false;
        return;
      }

      this.itemNumber++;
      
      if(this.itemNumber >= this.sequence.length) {
        this.round++;
        if(this.round > this.record)
          this.record = this.round;
        this.isGameDisabled = true;
        this.startGame();
        return;
      }        
    },
    
    addOpacity(color) {
      return `opacity: ${color ? '1' : '0.6'}`;
    },
    
    yellowClick() {
      this.yellowPush = !this.yellowPush;
      this.playAudio(1); 
      setTimeout(() => {
        this.yellowPush = !this.yellowPush;
      }, this.timeOfFlash/3);
    },
    blueClick() {
      this.bluePush = !this.bluePush;
      this.playAudio(2);
      setTimeout(() => {
        this.bluePush = !this.bluePush;
      }, this.timeOfFlash/3);
    },
    greenClick() {
      this.greenPush = !this.greenPush;
      this.playAudio(3);
      setTimeout(() => {
        this.greenPush = !this.greenPush;
      }, this.timeOfFlash/3);
    },
    redClick() {
      this.redPush = !this.redPush;
      this.playAudio(4);
      setTimeout(() => {
        this.redPush = !this.redPush;
      }, this.timeOfFlash/3);
    },
    playAudio(num) {
      switch(num) {
        case 1: 
          var simonSound1 = new Audio('https://s3.amazonaws.com/freecodecamp/simonSound1.mp3');
          simonSound1.play();
          return;
        case 2:
          var simonSound2 = new Audio('https://s3.amazonaws.com/freecodecamp/simonSound2.mp3');
          simonSound2.play();
          return;
        case 3:
          var simonSound3 = new Audio('https://s3.amazonaws.com/freecodecamp/simonSound3.mp3');
          simonSound3.play();
          return;
        case 4:
          var simonSound4 = new Audio('https://s3.amazonaws.com/freecodecamp/simonSound4.mp3');
          simonSound4.play();
          return;
      }

    
  },
    
  },
  computed: {
  }
}
</script>

<style>
h1,h2,h3,h4,h5,h6 {
  margin: 0;
}

body {
  width: 100%;
  height: 100%;
  margin: 0;
}

#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 80%;
  margin: 0 auto;
  box-sizing: border-box;
  padding: 0 15px;
}

.h1-title {
  margin-bottom: 40px;
}

.h2-title {
  font-size: clamp(1rem, 2vw , 1.8rem);
}

.h4-title {
  font-size: clamp(1rem, 2vw , 1.8rem);
  margin-top: 10px;
}

.h3-title {
  font-size: clamp(1rem, 2vw , 1.625rem);
  margin-bottom: 15px;
}

.simon{
  display: flex;
  justify-content: center;
  align-items: flex-start;
  gap: clamp(20px, 6vw, 150px);
}

.simon__game-place {
  flex: 0 0 clamp(200px, 25vw ,25vw);
}

.simon__interface {
  text-align: left;
  flex: 0 0 25%;
}

.simon__game {
  position: relative;
  background-color: rgb(151, 103, 103);
  height: clamp(200px, 25vw ,25vw);
  display: flex;
  flex-wrap: wrap;
  border-radius: 50%;
  overflow: hidden;
}

.game_item  {
  flex: 0 0 50%;
  height: 50%;
  border: none;
  cursor: pointer;
  transition: opacity 0.3s;
}
.game_item:disabled {
  cursor: not-allowed;
}

.simon__yellow {
  background-color: #ffff42;
}
.simon__blue {
  background-color: #3a1cff;
}
.simon__green {
  background-color: #66ff4d;
}
.simon__red {
  background-color: #ff3b1f;
}

.simon__start {
  font-size: clamp(1rem, 2vw , 1.625rem);
  padding: 0.7em 1.6em;
  color: #fff;
  background-color: rgb(77, 91, 216);
  border: none;
  cursor: pointer;
  border-radius: 20px;
  margin-top: 15px;
  margin-bottom: 15px;
  transition: background-color 0.3s;
}

.simon__start:hover {
  background-color: rgb(54, 70, 209);
}

.simon__start:disabled {
  background-color: rgb(204, 204, 204);
  cursor: not-allowed;
}

.simon__lose {
  margin: 0 0 20px 0;
  font-size: clamp(1rem, 2vw , 1.5rem);
  color: rgb(214, 28, 28);
  visibility: hidden;
  opacity: 0;
  transition: opacity 0.3s;
}
.simon__lose.valid {
  visibility: visible;
  opacity: 1;
}

.simon__radio {
  margin-bottom: 12px;
}

.radio {
  margin: 0 5px;
}

.radio-label {
  font-size: clamp(0.8rem, 2vw , 1.25rem);
}
</style>
