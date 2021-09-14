<template>
  <div class="hol-container">
    <h1>Higher Or Lower</h1>
    <div class="new-game">
        <button class="new-game-btn">Start New Game</button>
    </div>
    <div v-if="deck_id" class="game-area">
      <div class="row">
        <div class="deck-cnt">
          {{deck.remaining}}
        </div>
        <div class="score">
          <h3>Score: {{score}}</h3>
        </div>
      </div>
      <div class="row">
        <div class="player-card">

        </div>
        <div class="other-card">
          
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

    }
  }
}
</script>


<style scoped>
h1{
  color:black;
}
</style>
