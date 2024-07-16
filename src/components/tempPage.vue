<template>
    <div id="card-game">
      <h1 class="text-center p-5">Memory Game</h1>
      <h3 class="text-center">Score: {{ point }}</h3>
      <div class="container">
        <b-row class="justify-content-center">
          <b-col v-for="(card, index) in cards" :key="index" cols="auto" class="mb-3">
            <div class="flashcard" @click="toggleCard(card)">
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
      </div>
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
          { id: '5', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/cabbage.jpg'), flipped: false, matched: false },
          { id: '6', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/carrot.jpg'), flipped: false, matched: false },
          { id: '7', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/cauliflower.jpg'), flipped: false, matched: false },
          { id: '8', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/chilli.jpg'), flipped: false, matched: false },
          { id: '9', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/asparagus.jpg'), flipped: false, matched: false },
          { id: '10', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/avacado.jpg'), flipped: false, matched: false },
          { id: '11', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/beets.jpg'), flipped: false, matched: false },
          { id: '12', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/broccoli.jpg'), flipped: false, matched: false },
          { id: '13', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/cabbage.jpg'), flipped: false, matched: false },
          { id: '14', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/carrot.jpg'), flipped: false, matched: false },
          { id: '15', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/cauliflower.jpg'), flipped: false, matched: false },
          { id: '16', front: require('@/assets/Images/front.jpeg'), back: require('@/assets/Images/chilli.jpg'), flipped: false, matched: false },
        ],
        firstCard: null,
        secondCard: null,
        point: 0,
        showModal: false, // Control the visibility of the modal
      };
    },
    created() {
      this.shuffleCards();  // Shuffle the cards when the component is created
    },
    methods: {
      toggleCard(card) {
        // If two cards are already flipped, do nothing
        if (this.firstCard && this.secondCard) return;
  
        // Flip the card
        card.flipped = !card.flipped;
  
        // If it's the first card, store it
        if (!this.firstCard) {
          this.firstCard = card;
        } else {
          this.secondCard = card;
          if (this.firstCard.id === this.secondCard.id) {
            this.resetSelection();
            console.log("true");
          }
          // Check for a match
          if (this.firstCard.back === this.secondCard.back) {
            this.firstCard.matched = true;
            this.secondCard.matched = true;
            this.point += 1;  // Increment the point
            this.resetSelection();
  
            // Show modal if point reaches 8
            if (this.point === 8) {
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
      shuffleCards() {
        // Simple shuffle method using sort and Math.random
        this.cards.sort(() => Math.random() - 0.5);
      },
      resetSelection() {
        this.firstCard = null;
        this.secondCard = null;
      },
      resetGame() {
        // Reset game logic, shuffle cards and reset points
        this.shuffleCards();
        this.cards.forEach(card => {
          card.flipped = false;
          card.matched = false;
        });
        this.point = 0;
        this.showModal = false;
      },
    },
  };
  </script>
  
 