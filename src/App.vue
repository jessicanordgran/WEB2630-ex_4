<template>

  <div id="app">
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">PLAYER</h1>
        <div class="wins">
          <div class="wins text-center" style="background-color: green; margin: 0; color: white;":style="{width: playerWins + '0%'}">
            {{playerWins}}
          </div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">COMPUTER</h1>
        <div class="wins">
          <div class="wins text-center" style="background-color: green; margin: 0; color: white;" :style="{width: computerWins + '0%'}">
            {{computerWins}}
          </div>
        </div>
      </div>
    </section>

    <!--START GAME-->
    <section class="row controls" >
      <div class="small-12 columns">
        <button id="start-game"   @click="startGame">START NEW GAME</button>
      </div>
    </section>

    <!--ROCK PAPER SCISSORS Buttons-->
    <section v-show="showGame" class="row controls">
      <div class="small-12 columns">
        <button id="rock" @click="userRock">ROCK</button>
        <button id="paper" @click="userPaper">PAPER</button>
        <button id="scissors" @click="userScissors">SCISSORS</button>

        <!--RESTART button-->
        <button id="restart" @click="restartGame">RESTART</button>
      </div>
    </section>
    <section class="row log" v-show="showGame">
      <div class="small-12 columns">
        <ul>
          <li v-for="turn in turns"
              :class="{'player-turn': turn.isPlayer, 'computer-turn': !turn.isPlayer, 'tie-turn':turn.tie}">
            {{turn.text}}
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      show: true,
      playerWins: 0,
      computerWins: 0,
      // gameIsRunning: false,
      rock: 1,
      paper: 2,
      scissors: 3,
      turns: [],
      showGame: false,
    }
  },
      methods: {
      startGame: function(){
        // this.gameIsRunning = true;
        this.playerWins = 0;
        this.computerWins = 0;
        this.turns = [];
        this.showGame = true;
      },
      restartGame: function(){
        // this.gameIsRunning = false;
        this.playerWins = 0;
        this.computerWins = 0;
        this.turns = [];
        this.showGame = false;
      },
      userRock: function(){
        //generate computer choice
        var computer = this.computerChoice();
        //output of winner
        // paper
        if(computer === this.paper){
          this.computerWins += 1;
          this.turns.unshift({
            isPlayer: false,
            text: 'Computer Chose Paper| Paper Beats Rock| Computer Wins!'
          })
        }
        //scissors
        else if(computer === this.scissors){
          this.playerWins += 1;
          //this.playerWins = this.playerWins + 1;
          this.turns.unshift({
            isPlayer: true,
            text: 'Computer Chose Scissors | Rock Beats Scissors| Player Wins!'
          })
        }
        //TIE
        else{
          this.turns.unshift({
            tie: true,
            text: 'Computer Chose Rock | You Have Tied!'
          })
        }
        //check for winner
        this.checkWinner();
      },

      userPaper: function(){
        //generate computer choice
        var computer = this.computerChoice();
        //output of winner
        // scissors
        if(computer === this.scissors){
          this.computerWins += 1;
          this.turns.unshift({
            isPlayer: false,
            text: 'Computer Chose Scissors| Scissors Beats Paper| Computer Wins!'
          })
        }
        //scissors
        else if(computer === this.rock){
          this.playerWins += 1;
          //this.playerWins = this.playerWins + 1;
          this.turns.unshift({
            isPlayer: true,
            text: 'Computer Chose Rock | Paper Beats Rock| Player Wins!'
          })
        }
        //TIE
        else{
          this.turns.unshift({
            tie: true,
            text: 'Computer Chose Paper | You Have Tied!'
          })
        }
        //check for winner
        this.checkWinner();
      },

      userScissors: function(){
        //generate computer choice
        var computer = this.computerChoice();
        //output of winner
        // rock
        if(computer === this.rock){
          this.computerWins += 1;
          this.turns.unshift({
            isPlayer: false,
            text: 'Computer Chose Rock| Rock Beats Scissors| Computer Wins!'
          })
        }
        //paper
        else if(computer === this.paper){
          this.playerWins += 1;
          //this.playerWins = this.playerWins + 1;
          this.turns.unshift({
            isPlayer: true,
            text: 'Computer Chose Paper | Scissors Beats Paper| Player Wins!'
          })
        }
        //TIE
        else{
          this.turns.unshift({
            tie: true,
            text: 'Computer Chose Scissors | You Have Tied!'
          })
        }
        //check for winner
        this.checkWinner();
      },


      //random computer choice
      computerChoice: function(){
        var max = 3;
        var min = 0;
        return Math.max(Math.floor(Math.random() * max) + 1, min)
      },
      // paper: function(){
      //
      // },
      // scissors: function(){
      //
      // },




      //alert after hitting 10 and announcing winner
      checkWinner: function(){
        if(this.playerWins === 10){
          if(confirm('You Win! New Game.')){
            this.startGame();
          }else{
            this.gameIsRunning = false;
          }
        }
        else if (this.computerWins === 10){
          if(confirm('Computer Won! New Game.')){
            this.startGame();
          }else{
            this.gameIsRunning = false;
          }
        }
      }
    }
}
</script>


<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.text-center {
  text-align: center;
}

.wins {
  width: 80%;
  color: black;
  height: 40px;
  background-color: #eee;
  margin: auto;
  transition: width 1000ms;
}

.controls, .log {
  margin-top: 30px;
  text-align: center;
  padding: 10px;
  border: 1px solid #ccc;
  box-shadow: 0px 3px 6px #ccc;
}

.turn {
  margin-top: 20px;
  margin-bottom: 20px;
  font-weight: bold;
  font-size: 22px;
}

.log ul {
  list-style: none;
  font-weight: bold;
  text-transform: uppercase;
}

.log ul li {
  margin: 5px;
}

.log ul .player-turn {
  color: green;
  background-color: #aaffb0;
}

.log ul .computer-turn {
  color: red;
  background-color: #ffc0c1;
}

.log ul .tie-turn {
  color: blue;
  background-color: #e4e8ff;
}

button {
  font-size: 20px;
  background-color: #eee;
  padding: 12px;
  box-shadow: 0 1px 1px black;
  margin: 10px;
}

#start-game {
  background-color: #e4e8ff;
}

#start-game:hover {
  background-color: #687eff;
}

#rock {
  background-color: #ff7367;
}

#rock:hover {
  background-color: #ff3f43;
}

#paper {
  background-color: #ffaf4f;
}

#paper:hover {
  background-color: #ff9a2b;
}

#scissors {
  background-color: #aaffb0;
}

#scissors:hover {
  background-color: #76ff7e;
}

#restart {
  background-color: #ffffff;
}

#restart:hover {
  background-color: #c7c7c7;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
