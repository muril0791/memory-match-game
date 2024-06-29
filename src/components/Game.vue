<template>
    <div>
        <h1>Memory Match Game</h1>
        <div class="controls">
            <label for="difficulty">Difficulty:</label>
            <select id="difficulty" v-model="difficulty" @change="resetGame">
                <option value="easy">Easy</option>
                <option value="medium">Medium</option>
                <option value="hard">Hard</option>
            </select>
        </div>
        <div class="game-board">
            <Card v-for="(card, index) in cards" :key="index" :cardBack="themes[difficulty].cardBack"
                :cardFront="card.front" :isFlipped="card.isFlipped" @flip="onCardClicked(card)" />
        </div>
    </div>
</template>

<script>
import Card from './Card.vue';

export default {
    components: {
        Card
    },
    data() {
        return {
            cards: [],
            matchedCards: [],
            flippedCards: [],
            difficulty: 'easy',
            themes: {
                easy: { cardBack: '/easy/back-easy.png' },
                medium: { cardBack: '/medium/back-medium.png' },
                hard: { cardBack: '/hard/back-hard.png' },
            },
        };
    },
    mounted() {
        this.resetGame();
    },
    methods: {
        resetGame() {
            this.cards = [];
            this.matchedCards = [];
            this.flippedCards = [];

            const cardTextures = this.generateCardTextures(8);

            for (let i = 0; i < 16; i++) {
                this.cards.push({
                    front: cardTextures[i % cardTextures.length],
                    isFlipped: false,
                    matched: false
                });
            }

            this.shuffleCards();
        },
        generateCardTextures(pairCount) {
            const textures = [];
            for (let i = 0; i < pairCount; i++) {
                const texture = `/card/front${i}.png`;
                textures.push(texture);
            }
            return textures;
        },
        shuffleCards() {
            for (let i = this.cards.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [this.cards[i], this.cards[j]] = [this.cards[j], this.cards[i]];
            }
        },
        onCardClicked(card) {
            if (this.flippedCards.length < 2 && !this.flippedCards.includes(card) && !card.matched) {
                card.isFlipped = true;
                this.flippedCards.push(card);

                if (this.flippedCards.length === 2) {
                    this.checkForMatch();
                }
            }
        },
        checkForMatch() {
            const [card1, card2] = this.flippedCards;
            if (card1.front === card2.front) {
                card1.matched = true;
                card2.matched = true;
                this.matchedCards.push(card1, card2);
                this.flippedCards = [];

                if (this.matchedCards.length === this.cards.length) {
                    setTimeout(() => alert('You won!'), 500);
                }
            } else {
                setTimeout(() => {
                    card1.isFlipped = false;
                    card2.isFlipped = false;
                    this.flippedCards = [];
                }, 1000);
            }
        }
    }
};
</script>

<style>
.controls {
    margin-bottom: 20px;
}

.game-board {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 10px;
    justify-items: center;
    align-items: center;
}
</style>
