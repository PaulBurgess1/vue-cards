<template>
  <div class="container-fluid p-0 m-0 bg-dark">
    <div class="row m-0">
        

      <div class="col pt-1 m-auto">
        <h1>Higher Or Lower</h1>
        <p>A simple card game.</p>
        <br>
        <button class="btn btn-lg btn-outline-info" @click="newGame">Start A New Game</button>
      </div>
    </div>
      <div class="row instruc-box">
        <div class="col">
          <div class="instructions">
          <h4>How to Play</h4>
          <p>The objective of the game is correctly guess if your card is higher or lower than the other card.</p>
          <p> <u>Card are ranked in this order:</u> </p>
          <p>K > Q > J >10 > 9 ... 3 > 2 > A</p>
          <p><u>However:</u></p>
          <p>A > K</p>
          <p>You get 1 point if you correctly guess if its higher or lower, and 5 points if you correctly guess that it is equal.</p>
        </div>
        </div>

    </div>
    <div class="bg-dark">
      <div class="alert alert-primary fade in show m-0" id="alert_end" style="display: none;">
                <h1>
                  <strong><i class="fas fa-award"></i></strong>
                  You Finished with a Score of {{this.score}}, Congratulations!
                  </h1> 
          </div>
    </div>
    
    <div v-if="deck_id" class="game-area bg-dark">
      <div class="row p-0 m-0">
        
        <div class="col deck-cnt">
         <h3>
           Cards Remaining: {{deck.remaining}}
           </h3> 
        </div>
        <div class="col score">
          <h3>Score: {{score}}</h3>
        </div>
        
      </div>
      <div class="row">
        <div class="col">
          <p>Deck ID: {{deck_id}}</p>
        </div>
      </div>
      <div class="row p-0">
        <div class="col alert-box">
          <div class="alert alert-success fade in show" id="alert_succ" style="display: none;">
                <h5><strong><i class="fas fa-surprise"></i></strong> You Guessed Correct!</h5> 
          </div>
          <div class="alert alert-danger fade in show" id="alert_dang" style="display: none;">
               <h5>
                 <strong><i class="fas fa-grin-beam-sweat"></i></strong> You Guessed Wrong...
                </h5> 
          </div>
        </div>
        
      </div>
      <div class="row p-0 m-0">
        <div class="col player-card">
          <h4>Your Card</h4>
            <img class="card-img" :src="deck.cards[deck.cards.length - 2].image" alt="">
        </div>
        <div class="col actions">
          <span v-if="!action_flag" class="action-box"> 
            <button class="btn btn-lg btn-outline-primary" @click="cardAction(deck.cards[deck.cards.length - 2].value[0], deck.cards[deck.cards.length - 1].value[0], 'H')"><i class="fas fa-chevron-up"></i></button>
            <br>
            <button class="btn btn-lg btn-outline-primary" @click="cardAction(deck.cards[deck.cards.length - 2].value[0], deck.cards[deck.cards.length - 1].value[0], 'E')"><i class="fas fa-equals"></i></button>
            <br>
            <button class="btn btn-lg btn-outline-primary" @click="cardAction(deck.cards[deck.cards.length - 2].value[0], deck.cards[deck.cards.length - 1].value[0], 'L')"><i class="fas fa-chevron-down"></i></button>
          </span>
          
          <button class="btn btn-lg btn-outline-info w-100" v-if="action_flag" @click="nextRound">Draw</button>
        </div>
        <div class="col other-card">
          <h4>Other Card</h4>
          <img v-if="action_flag" class="card-img" :src="deck.cards[deck.cards.length - 1].image" alt="">
          <img v-else class="card-img" src="../assets/card-back.png" alt="">
          
        </div>
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  name: 'HighOrLow',
  data(){
    return{
      API_URL: "https://deckofcardsapi.com/api/deck/",
      deck: "",
      deck_id: "",
      score: 0,
      action_flag: 0,
      CARD_RANK: ['A', "2" ,"3" , "4", "5", "6", "7", "8", "9", "1", "J", "Q", "K"]
    }
  },
  methods:{
    newGame(){
      document.getElementById("alert_end").setAttribute("style","display: none;");
      this.score=0;
      try {
          fetch(this.API_URL+"new/draw/?count=2")
          .then(res => {
            if (res.status == 200){
                let data =res.json();
                //console.log(data)
                return data;
              }
              else{
                console.log("error")
                alert("Error "+res.status+": "+res.statusText);
                return;
              }
          })
          .then(this.setDeck);
        } catch (error) {
          alert(error.message);
        }
    },
    setDeck(data){
      this.deck = data;
      this.deck_id = this.deck.deck_id;
    },
    drawTwoCards(){
      try {
          fetch(this.API_URL+ this.deck_id+"/draw/?count=2")
          .then(res => {
            if (res.status == 200){
                let data =res.json();
                //console.log(data)
                return data;
              }
              else{
                console.log("error")
                alert("Error "+res.status+": "+res.statusText);
                return;
              }
          })
          .then(this.setDeck);
        } catch (error) {
          alert(error.message);
        }
    },
    isCorrect(player_card, other_card, action){
      if(action === "E"){
        if (player_card === other_card){
          this.score +=5;
          return true;
        }
        return false;
      }//=
      else if(action === "H"){
        if(player_card==="A" && other_card==="K"){//Player Ace, Other King Case
            this.score+=1;
            return true;
        }
        else if(player_card==="K" && other_card==="A"){//Other Ace, Player King Case
          return false
        }
        else{//Standard Case
          let p_val = this.CARD_RANK.indexOf(player_card);
          let o_val = this.CARD_RANK.indexOf(other_card);
          if (p_val > o_val){
            this.score+=1;
            return true;
          }
          else{
            return false;
          }
        }
      }
      else{//action === "L"
        if(player_card==="A" && other_card==="K"){//Player Ace, Other King Case
          return false;
        }
        else if(player_card==="K" && other_card==="A"){//Other Ace, Player King Case
          this.score+=1;
          return true;
        }
        else{//Standard Case
          let p_val = this.CARD_RANK.indexOf(player_card);
          let o_val = this.CARD_RANK.indexOf(other_card);
          if (p_val < o_val){
            this.score+=1;
            return true;
          }
          else{
            return false;
          }
        }

      }
      

    },
    cardAction(player_card, other_card, action){
      let result = this.isCorrect(player_card, other_card, action);
      this.action_flag=1;
      if (result){
        document.getElementById("alert_succ").setAttribute("style","display: default;");
      }
      else{
        document.getElementById("alert_dang").setAttribute("style","display: default;");
      }
    },//cardAction
    nextRound(){
      this.action_flag=0;
      document.getElementById("alert_succ").setAttribute("style","display: none;");
      document.getElementById("alert_dang").setAttribute("style","display: none;");
      if(this.deck.remaining > 0){
        this.drawTwoCards();
      }
      else{
        this.deck_id="";
        document.getElementById("alert_end").setAttribute("style","display: default;");
      }
    }

    

  }
}
</script>


<style scoped>
.game-area{
  padding: 1rem 0 2rem 0;
  margin-top: 1rem;
  color: var(--clr-text);
  text-align: center;

}

.instruc-box{
  padding-top: 2rem;
  padding-bottom: 2rem;
}
.instructions{
  margin: 1rem;
  border: 1px solid azure;
  width: fit-content;
  margin:auto;
  box-shadow: rgba(255, 255, 255, 0.4) 5px 5px, rgba(255, 255, 255, 0.3) 10px 10px, rgba(255, 255, 255, 0.2) 15px 15px, rgba(255, 255, 255, 0.1) 20px 20px, rgba(255, 255, 255, 0.05) 25px 25px;
}
.instructions > h4{
  padding-top: 0.5rem;
  padding-bottom: 0.3rem;
  border: 1px solid azure;
  
}
.instructions > p{
  padding:0;
  margin: 0;
}
.actions{
  margin:auto;
  padding:0;
  
}
.card-img{
  max-height: 14rem;
  width:auto;
  box-shadow: -5px -5px 10px 3px red, 5px 5px 10px 3px blue;
}
.player-card{
  align-content: right;
}
.alert-box{
  min-height: 5rem;
}
</style>
