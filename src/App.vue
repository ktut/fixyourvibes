<script>
import ContactForm from './components/ContactForm.vue'

export default {
  name: 'App',
  components: {
    ContactForm
  },
  data() {
    return {
      glitchText: '$$$',
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
        this.glitchText = Array.from({ length: 3 }, (_, index) => {
          // Only the position at glitchIndex shows its special character
          // The other positions show the correct letter
          if (index === this.glitchIndex) {
            return letters[index].special
          }
          return letters[index].correct
        }).join('')

        // Move to next position
        this.glitchIndex = (this.glitchIndex + 1) % 3
      }, 1000)
    }
  }
}
</script>

<template>
  <div class="app">
    <main class="hero">
      <h1>Your bull<span class="glitch">{{ glitchText }}</span>t vibe-coded app is going to break in production.</h1>
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
  align-items: center;
  padding: $spacing-md;
  text-align: center;
  max-width: $width-hero-max;
  margin: 0 auto;

  h1 {
    font-size: clamp(2rem, 5vw, 4rem);
    font-weight: 800;
    margin-bottom: $spacing-sm;
    line-height: 1.2;
    background: $gradient-primary;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;

    .glitch {
      display: inline-block;
      background: $gradient-primary;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
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
