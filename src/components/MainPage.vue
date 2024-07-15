<template>
    <div id="card-game">
        <!-- title -->
      <h1 class="text-center p-5">Memory Game</h1>
      <!-- score points -->
      <h3 class="text-center">Score: {{ point }}</h3>
      <!-- display moves -->
      <h3 class="text-center">Current Moves: {{ moves }}</h3>
      <!-- display avilable moves -->
      <h3 class="text-center">Avilablemoves: {{ avilablemoves }}</h3>
      <b-container>
        <b-row class="justify-content-center" >
          <b-col v-for="(card, index) in cardscopy" :key="index" cols="3" class="mb-3">
            <!-- card container -->
            <div class="flashcard" @click="toggleCard(card)">
                <!-- adding flip animations -->
              <transition name="flip">
                <img
                  :src="card.flipped ? card.back : card.front"
                  :alt="card.flipped ? 'Back' : 'Front'"
                  class="card img-fluid"
                  :class="{ matched: card.matched }"
                />
              </transition>
            </div>
          </b-col>
        </b-row>
    </b-container>
      <!-- Modal -->
      <b-modal v-model="showModal" title="Congratulations!" @ok="resetGame">
        You won the game!
      </b-modal>
    </div>
  </template>
  
  <script>
  export default {
    name: 'MainPage',
    data() {
      return {
        cards: [
          { id: '1', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/asparagus.jpg'), flipped: false, matched: false },
          { id: '2', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/avacado.jpg'), flipped: false, matched: false },
          { id: '3', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/beets.jpg'), flipped: false, matched: false },
          { id: '4', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/broccoli.jpg'), flipped: false, matched: false },
          { id: '5', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/chilli.jpg'), flipped: false, matched: false },
          { id: '6', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/carrot.jpg'), flipped: false, matched: false },
        
        //   { id: '7', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/asparagus.jpg'), flipped: false, matched: false },
        //   { id: '8', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/avacado.jpg'), flipped: false, matched: false },
        //   { id: '9', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/beets.jpg'), flipped: false, matched: false },
        //   { id: '10', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/broccoli.jpg'), flipped: false, matched: false },
        //   { id: '11', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/chilli.jpg'), flipped: false, matched: false },
        //   { id: '12', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/carrot.jpg'), flipped: false, matched: false },
          
        ],
        cardscopy:[],
        firstCard: null,
        secondCard: null,
        point: 0,
        moves: 0,
        totalmoves: 15,
        avilablemoves: 15,
        showModal: false, 
      };
    },
    created() {
        
        this.cardscopy = [  ...this.cards,  ...this.cards.map(card => ({...card, }))];



  // Shuffle the cards when the component is created
  this.shuffleCards(); 
    },
    methods: {
        //card toggle function
      toggleCard(card) {

        // If two cards are already flipped, do nothing
        if (this.firstCard && this.secondCard) return;

        if (card=== this.firstCard) return;        
  
        // Flip the card
        card.flipped = !card.flipped;
  
        // If it's the first card, store it
        if (!this.firstCard) {
          this.firstCard = card;
          this.moves += 1;
          this.avilablemoves=this.totalmoves-this.moves;
          if(this.avilablemoves===0){
            alert("all moves are over")
            this.resetGame()
          }
        } else {
          this.secondCard = card;
        //   if (this.firstCard.id === this.secondCard.id) {
        //     this.resetSelection();
            
        //   }
          // Check for a match
          if (this.firstCard.back === this.secondCard.back) {
            this.firstCard.matched = true;
            this.secondCard.matched = true;
            this.point += 1;  // Increment the point
            this.resetSelection();
  
            // Show modal if point reaches 8
            if (this.point === 6) {
              this.showModal = true;
            }
          } else {
            // If no match, flip the cards back after a short delay
            setTimeout(() => {
              this.firstCard.flipped = false;
              this.secondCard.flipped = false;
              this.resetSelection();
            }, 1000);
          }
        }
      },
      // Simple shuffle method using sort and Math.random
      shuffleCards() {
        this.cardscopy.sort(() => Math.random() - 0.5);
      },
      //rest card selection
      resetSelection() {
        this.firstCard = null;
        this.secondCard = null;
      },
      // Reset game logic, shuffle cards and reset points
      resetGame() {
        this.shuffleCards();
        this.cardscopy.forEach(card => {
          card.flipped = false;
          card.matched = false;
        });
        this.point = 0;
        this.showModal = false;
        this.moves=0;
        this.avilablemoves=15;
      },
    },
  };
  </script>
  
 