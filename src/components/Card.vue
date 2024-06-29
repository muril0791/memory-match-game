<template>
    <div class="card" :class="{ flipped: isFlipped }" @click="flipCard">
        <img :src="cardImage" :alt="isFlipped ? 'card front' : 'card back'" />
    </div>
</template>

<script>
export default {
    props: {
        cardBack: String,
        cardFront: String,
        isFlipped: Boolean
    },
    computed: {
        cardImage() {
            return this.isFlipped ? this.cardFront : this.cardBack;
        }
    },
    methods: {
        flipCard() {
            this.$emit('flip');
        }
    }
};
</script>

<style>
.card {
    width: 100px;
    height: 100px;
    margin: 5px;
    cursor: pointer;
    perspective: 1000px;
}

.card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.6s;
    transform-style: preserve-3d;
}

.card.flipped .card-inner {
    transform: rotateY(180deg);
}

.card-front,
.card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    border-radius: 10px;
    overflow: hidden;
}

.card-back {
    transform: rotateY(180deg);
}

.card img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}
</style>
