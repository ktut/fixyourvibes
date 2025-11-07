<script>
import ContactForm from './components/ContactForm.vue'

export default {
  name: 'App',
  components: {
    ContactForm
  },
  data() {
    return {
      glitchText: [
        { char: '$', key: 0 },
        { char: 'h', key: 1 },
        { char: 'i', key: 2 }
      ],
      animationInterval: null,
      glitchIndex: 0 // Track which position shows special character
    }
  },
  mounted() {
    this.startAnimation()
  },
  beforeUnmount() {
    if (this.animationInterval) {
      clearInterval(this.animationInterval)
    }
  },
  methods: {
    startAnimation() {
      const letters = [
        { correct: 's', special: '$' },
        { correct: 'h', special: '%' },
        { correct: 'i', special: '!' }
      ]

      this.animationInterval = setInterval(() => {
        this.glitchText = this.glitchText.map((item, index) => {
          // Only the position at glitchIndex shows its special character
          // The other positions show the correct letter
          const newChar = index === this.glitchIndex
            ? letters[index].special
            : letters[index].correct

          // Only update if character changed
          if (newChar !== item.char) {
            return {
              char: newChar,
              // Only increment key (trigger animation) when changing TO special character
              key: index === this.glitchIndex ? item.key + 3 : item.key
            }
          }
          return item
        })

        // Move to next position
        this.glitchIndex = (this.glitchIndex + 1) % 3
      }, 3000)
    }
  }
}
</script>

<template>
  <div class="app">
    <main class="hero">
      <h1>Your bull<span class="glitch"><span
          v-for="item in glitchText"
          :key="item.key"
          class="glitch-char"
        >{{ item.char }}</span></span>t <br class="break-mobile"/>vibe-coded app <br class="break-mobile"/>is going to break in production.</h1>
      <h2>We can keep that from happening.</h2>
    </main>

    <ContactForm />
  </div>
</template>

<style lang="scss">
@import './styles/global.scss';
@import './styles/variables.scss';

.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.hero {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: $spacing-md;
  text-align: center;
  max-width: $width-hero-max;
  margin: 0 auto;
  min-height: 100vh;
  @media (min-width: 768px) {
    align-items: center;
  }

  h1,
  h2 {
    text-align: left;
    @media (min-width: 768px) {
      text-align: center;
      .break-mobile {
        display: none;
      }
    }
  }

  h1 {
    font-size: clamp(2.5rem, 6vw, 6rem);
    font-weight: 800;
    margin-bottom: $spacing-sm;
    line-height: 1.2;
    background: $gradient-primary;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;

    .glitch {
      display: inline-block;
      perspective: 1000px;

      .glitch-char {
        display: inline-block;
        animation: flip3d 0.6s ease-in-out;
        transform-style: preserve-3d;
        background: $gradient-primary;
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
      }
    }

    @keyframes flip3d {
      0% {
        transform: rotateX(0deg);
        opacity: 1;
      }
      50% {
        transform: rotateX(90deg);
        opacity: 0;
      }
      51% {
        transform: rotateX(-90deg);
        opacity: 0;
      }
      100% {
        transform: rotateX(0deg);
        opacity: 1;
      }
    }
  }

  h2 {
    font-size: clamp(1.2rem, 3vw, 2rem);
    font-weight: 400;
    color: $text-secondary;
    max-width: $width-container-max;
  }
}
</style>
