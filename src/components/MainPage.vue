<template>          
    <div id="card-game">
    <!-- heading container -->
    <div class="heading-container">
        <h1 class="text-center ">Memory Game</h1>
        <div  class="moves-container">
            <div>
        
            <h3 class="ml-5">Current Moves: {{ moves }}</h3>
        <!-- display available moves -->
            <h3 class="ml-5">Available Moves: {{ avilableMoves }}</h3>
        </div>
        <div class="score-container"> 
            <h3 class="">Score</h3>
            <h3 class=""> {{ point }}</h3>

        </div>
        </div>
    </div>
    <!-- BG-IMG -->
    <div class="image-url">
        
    </div>
    <b-container>  
        <b-row class="justify-content-center">
          <b-col
            v-for="(card, index) in cardsCopy"
            :key="index"
            cols="12"
            sm="6"
          md="3"
            class="mb-3 d-flex justify-content-center"
          >
            <!-- card container -->
            
            <div class="flip-card" @click="toggleCard(card)">
    <div class="flip-card-inner" :class="{ flipped: card.flipped }">
      <div class="flip-card-front">
        <img
          :src="card.front"
          :alt="'Front of ' + card.name"
          class="card shadow-animate img-fluid"
        />
      </div>
      <div class="flip-card-back">
        <img
          :src="card.back"
          :alt="'Back of ' + card.name"
          class="card shadow-animate img-fluid"
           :class="{ matched: card.matched }"
        />
      </div>
    </div>
  </div>

          </b-col>
        </b-row>
    </b-container>
      <!-- Modal -->
    <b-modal v-model="showModal" title="Congratulations!" @ok="resetGame" hide-header-close ok-only>
        <CelebrationPage />
    </b-modal>
    </div>

  </template>
  
  <script>
  import CelebrationPage from '@/components/CelebrationPage.vue';

  export default {
    
    name: 'MainPage',
    components: {
        CelebrationPage,
  },
    data() {
      return {
        cards: [
          { id: '1', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/cat.jpeg'), flipped: false, matched: false },
          { id: '2', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/elephant.jpeg'), flipped: false, matched: false },
          { id: '3', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/lion.jpeg'), flipped: false, matched: false },
          { id: '4', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/panda.jpeg'), flipped: false, matched: false },
          { id: '5', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/dog.jpeg'), flipped: false, matched: false },
          { id: '6', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/cock.jpeg'), flipped: false, matched: false },
        
          
        ],
        cardsCopy:[],// initialize array 
        firstCard: null, // assigning firstcard as 'null' in initial stage
        secondCard: null, // assigning secondcard as 'null' in initial stage
        point: 0,// assigning point as '0' in initial stage
        moves: 0,// assigning moves as '0' in initial stage
        totalMoves: 15,// assigning totalmoves as '15' in initial stage
        avilableMoves: 15, // assigning avilable moves as '15' in initial stage
        showModal: false, 
      };
    },
    created() {
        //make duplicate copies of original array
        this.cardsCopy = [  ...this.cards,  ...this.cards.map(card => ({...card, }))];
        // Shuffle the cards when the component is created
        this.shuffleCards(); 
    },


    methods: {
        //card toggle function
      toggleCard(card) {
        if(this.avilableMoves===0) return;
        // If two cards are already flipped, do nothing
        if (this.firstCard && this.secondCard) return;

        if (card=== this.firstCard) return;        
  
        // Flip the card
        card.flipped = !card.flipped;
  
        // If it's the first card, store it
        if (!this.firstCard) {
          this.firstCard = card;
         
        } else {
           // If it's the second card, store it
          this.secondCard = card;
           // update avilable moves
           this.moves += 1; // increment moves

            this.avilableMoves=this.totalMoves-this.moves;
            if(this.avilableMoves == 0){
                setTimeout(() => {
                    alert("all moves are over")
                    this.resetGame()
                }, 2000)
                        
            }

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
        this.cardsCopy.sort(() => Math.random() - 0.5);
      },
      //rest card selection
      resetSelection() {
        this.firstCard = null;
        this.secondCard = null;
      },
      // Reset game logic, shuffle cards and reset points
      resetGame() {
        this.shuffleCards();
        this.cardsCopy.forEach(card => {
          card.flipped = false;
          card.matched = false;
        });
        this.point = 0;
        this.showModal = false;
        this.moves=0;
        this.avilableMoves=15;
      },
    },
  };
  </script>

